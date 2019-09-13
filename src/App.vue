<template>
  <div id="app">
    <Header />
    <AutoCom0 />
    <AutoComplete />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import AutoComplete from './components/AutoComplete.vue'
import AutoCom0 from './components/AutoCom0.vue'

export default {
  name: 'app',
  components: {
    AutoComplete,
    AutoCom0,
    Header
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}
</style>
