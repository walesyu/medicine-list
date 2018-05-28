<template>
  <div id="app">
    <Prescriptions/>
  </div>
</template>

<script>
import Prescriptions from './components/Prescriptions'

export default {
  name: 'App',
  components: {
    Prescriptions
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
