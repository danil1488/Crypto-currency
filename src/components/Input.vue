<template>
  <input
    min="0"
    type="number"
    @input="changeAmount($event.target.value)"
    placeholder="Введите число"
  /><br />
  <button @click="convert()">CONVERT CURRENCY</button>
</template>

<script>
export default {
  props: {
    changeAmount: {
      type: Function,
      required: true,
    },
    convert: {
      type: Function,
      required: true,
    },
  },
}
</script>

<style scoped>
input {
  outline: none;
  width: 80%;
  max-width: 500px;
  font-size: 1.2em;
  border-radius: 3px;
  border: 1px solid #ccc;
  padding: 10px;
  background: #fafafa;
  opacity: 0.9;
}

button {
  font-family: 'Tomorrow', sans-serif;
  padding: 10px 15px;
  color: #fff;
  background: #1a032d;
  border-radius: 6px;
  border: none;
  font-size: 1em;
  width: auto;
  margin-top: 10px;
  text-transform: uppercase;
  cursor: pointer;
  opacity: 0.8;
}

/* Адаптивность для мобильных устройств */
@media (max-width: 768px) {
  input {
    width: 90%;
    font-size: 1em;
  }

  button {
    width: 90%;
    font-size: 0.9em;
  }
}
</style>
