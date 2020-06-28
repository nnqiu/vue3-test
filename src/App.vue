<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- <Setup /> -->
    <!-- <Computed /> -->
    <!-- <Watch /> -->
    <Refs />
    <button @click="increment">
      Count is: {{ state.count }}, double is: {{ state.double }}
    </button>
  </div>
</template>

<script>
// import Setup from './components/Setup.vue'
// import Computed from './components/Computed.vue'
// import Watch from './components/Watch.vue'
import Refs from './components/Refs.vue'

import { reactive, computed } from 'vue';


export default {
  name: 'App',
  components: {
    // Setup,
    // Computed
    // Watch
    Refs
  },
  setup() {
    const state = reactive({
      count: 0,
      double: computed(() => state.count * 2),
    })

    function increment() {
      state.count++
    }

    return {
      state,
      increment,
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
