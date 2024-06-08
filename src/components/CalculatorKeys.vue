<template>
  <div class="keys">
    <div v-for="key in keys" :key="key.value" :class="['key', key.type]" @click="handleClick(key.value)">
      <span>{{ key.display }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      keys: [
        { value: "clear", display: "AC", type: "action" },
        { value: "brackets", display: "( )", type: "action" },
        { value: "%", display: "%", type: "action" },
        { value: "/", display: "÷", type: "operator" },
        { value: "7", display: "7" },
        { value: "8", display: "8" },
        { value: "9", display: "9" },
        { value: "*", display: "x", type: "operator" },
        { value: "4", display: "4" },
        { value: "5", display: "5" },
        { value: "6", display: "6" },
        { value: "-", display: "-", type: "operator" },
        { value: "1", display: "1" },
        { value: "2", display: "2" },
        { value: "3", display: "3" },
        { value: "+", display: "+", type: "operator" },
        { value: "backspace", display: "<", type: "action" },
        { value: "0", display: "0" },
        { value: ".", display: "." },
        { value: "=", display: "=", type: "operator" }
      ]
    };
  },
  methods: {
    handleClick(value) {
      this.$emit('keyPressed', value);
    }
  }
};
</script>
  
<style scoped>
.keys {
  background-color: #292d36;
  padding: 1.5rem;
  border-radius: 1.5rem;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 80px;
  grid-gap: 10px;
}

.keys .key {
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #262933;
  border-radius: 1rem;
  transition: 0.2s;
}

.keys .key span {
  font-size: 2rem;
  font-weight: 700;
  color: #fff;
}

.keys .key:hover {
  box-shadow: inset 0px 0px 8px rgba(0, 0, 0, 0.2);
}

.keys .key.operator span {
  color: #eb6666;
}

.keys .key.action span {
  color: #26fed7;
}
</style>
