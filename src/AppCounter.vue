<template>
  <h1 ref="header">App Counter</h1>
  <CounterValue
    class="counter"
    v-for="obj in [{ index: 1, text: 'Clicked' }]"
    :title="obj.text"
    :value="counter"
    :key="obj.index"
  />
  <button v-on:click="onPlus">+</button>
  <button v-if="canRenderMinusButton" @click="onMinus">-</button>
</template>
<script>
import CounterValue from "./components/CounterValue.vue";

const LOCAL_KEY_COUNTER = "counter";
const saveCounter = (value) => localStorage.setItem(LOCAL_KEY_COUNTER, value);
let counterWatcher = null;

export default {
  components: {
    CounterValue,
  },
  data() {
    return {
      counter: 0,
    };
  },
  created() {
    console.log("> created: ", this.counter);
    this.counter = localStorage.getItem("counter") || 0;
    counterWatcher = this.$watch(
      () => this.counter,
      (newValue, oldValue) => {
        console.log("> counter watched:", { newValue, oldValue });
        saveCounter(newValue);
      }
    );
  },
  mounted() {
    console.log("> mounted: ", this.counter);
  },
  computed: {
    canRenderMinusButton() {
      return this.counter > 0;
    },
  },
  methods: {
    onPlus() {
      this.counter++;
      console.log("> Counter -> onPlus", this.counter);
    },
    onMinus() {
      this.counter--;
      if (this.counter === 0) {
        this.$refs.header.innerText = `Header: ${this.counter}`;
      }
      console.log("> Counter -> onMinus", this.counter);
    },
  },
  unmounted() {
    counterWatcher();
  },
  components: { CounterValue },
};
</script>
<style lang="scss" scoped>
.counter {
  color: green;
}
</style>
