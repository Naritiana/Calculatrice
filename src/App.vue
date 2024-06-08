<template>
  <div class="app">
    <div class="calculator">
      <CalculatorDisplay :input="input" :output="output" />
      <CalculatorKeys @keyPressed="handleClick" />
    </div>
  </div>
</template>

<script>
  import CalculatorDisplay from './components/CalculatorDisplay.vue';
  import CalculatorKeys from './components/CalculatorKeys.vue';

  export default {
    data() {
      return {
        input: "",
        output: "",
      };
    },
    methods: {
      handleClick(value) {
        if (value === "clear") {
          this.input = "";
          this.output = "";
        } else if (value === "backspace") {
          this.input = this.input.slice(0, -1);
        } else if (value === "=") {
          this.calculateResult();
        } else if (value === "brackets") {
          this.handleBrackets();
        } else if (value === "π") {
          this.input += "π";
        } else if (value === "sqrt") {
          this.input += "Math.sqrt(";
        } else if (value === "square") {
          this.input += "^2";
        } else {
          if (this.validateInput(value)) {
            this.input += value;
          }
        }
      },
      handleBrackets() {
        const lastChar = this.input.slice(-1);
        const openBrackets = (this.input.match(/\(/g) || []).length;
        const closeBrackets = (this.input.match(/\)/g) || []).length;

        if (openBrackets > closeBrackets && (/\d$/.test(lastChar) || lastChar === ')')) {
          this.input += ")";
        } else {
          if (lastChar === "" || ["+", "-", "*", "/", "("].includes(lastChar)) {
            this.input += "(";
          } else {
            this.input += "*(";
          }
        }
      },
      calculateResult() {
        try {
          // Convertir les degrés en radians
          let input = this.input.replace(/sin\(([^)]+)\)/g, (match, p1) => `sin(${p1 * (Math.PI / 180)})`);
          input = input.replace(/cos\(([^)]+)\)/g, (match, p1) => `cos(${p1 * (Math.PI / 180)})`);

          const evalInput = input.replace(/π/g, Math.PI);
          const result = eval(evalInput);
          if (isNaN(result) || !isFinite(result)) {
            throw new Error("Invalid input");
          }
          this.output = result.toString();
        } catch (e) {
          this.output = "Error";
        }
      },
      prepareInput(input) {
        const preparedInput = input.replace(/%/g, "/100");
        return preparedInput;
      },
      validateInput(value) {
        const lastInput = this.input.slice(-1);
        const operators = ["+", "-", "*", "/"];
        if (value === "." && lastInput === ".") return false;
        if (operators.includes(value) && operators.includes(lastInput)) return false;
        return true;
      }
    },
    components: {
      CalculatorDisplay,
      CalculatorKeys
    }
  };
</script>