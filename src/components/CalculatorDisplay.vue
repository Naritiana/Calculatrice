<template>
  <div class="display">
    <div class="content">
      <div class="input" v-html="cleanInput"></div>
      <div class="output">{{ output }}</div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['input', 'output'],
    computed: {
      cleanInput() {
        return this.input
          .split("")
          .map(char => {
            if (char === "^") return ` <span class="operator">²</span> `; 
            if (char === "*") return ` <span class="operator">x</span> `;
            if (char === "/") return ` <span class="operator">÷</span> `;
            if (char === "+") return ` <span class="operator">+</span> `;
            if (char === "-") return ` <span class="operator">-</span> `;
            if (char === "(") return `<span class="brackets">(</span>`;
            if (char === ")") return `<span class="brackets">)</span>`;
            if (char === "%") return `<span class="percent">%</span>`;
            return char;
          })
          .join("");
      },
    }
  };
</script>

<style scoped>
  .display {
    min-height: 200px;
    padding: 1.5rem;
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    color: white;
    text-align: right;
    flex: 1 1 0%;
  }

  .display .content {
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    width: 100%;
    max-width: 100%;
    overflow: auto;
  }

  .display .input {
    width: 100%;
    font-size: 1.25rem;
    margin-bottom: 0.5rem;
  }

  .display .output {
    font-size: 3rem;
    font-weight: 700;
    width: 100%;
    white-space: nowrap;
  }

  .display .operator {
    color: #eb6666;
  }

  .display .brackets,
  .display .percent {
    color: #26fed7;
  }
</style>