<template>
  <CalcComp/>
</template>

<script>
import CalcComp from './components/CalcComp.vue'

export default {
  name: 'App',
  components: {
    CalcComp,
  }
}
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
