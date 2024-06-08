<template>
  <div class="app">
    <div class="calculator">
      <CalculatorDisplay :input="input" :output="cleanOutput" />
      <CalculatorKeys @keyPressed="handleClick" />
    </div>
  </div>
</template>

<script>
import CalculatorDisplay from './components/CalculatorDisplay.vue';
import CalculatorKeys from './components/CalculatorKeys.vue';
import './assets/styles.css'; // Importer le fichier CSS global

export default {
  data() {
    return {
      input: "",
    };
  },
  computed: {
    cleanOutput() {
      if (!this.input) return "";
      try {
        let result = eval(this.prepareInput(this.input));
        return result.toLocaleString();
      } catch (e) {
        return "";
      }
    }
  },
  methods: {
    handleClick(value) {
      if (value === "clear") {
        this.input = "";
      } else if (value === "backspace") {
        this.input = this.input.slice(0, -1);
      } else if (value === "=") {
        this.input = eval(this.prepareInput(this.input)).toString();
      } else if (value === "brackets") {
        if (
          this.input.indexOf("(") === -1 ||
          (this.input.indexOf("(") !== -1 &&
            this.input.indexOf(")") !== -1 &&
            this.input.lastIndexOf("(") < this.input.lastIndexOf(")"))
        ) {
          this.input += "(";
        } else if (
          this.input.indexOf("(") !== -1 &&
          this.input.indexOf(")") === -1 ||
          (this.input.indexOf("(") !== -1 &&
            this.input.indexOf(")") !== -1 &&
            this.input.lastIndexOf("(") > this.input.lastIndexOf(")"))
        ) {
          this.input += ")";
        }
      } else {
        if (this.validateInput(value)) {
          this.input += value;
        }
      }
    },
    prepareInput(input) {
      return input.replace(/%/g, "/100");
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
