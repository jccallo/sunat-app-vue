<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  <edit-client></edit-client>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import EditClient from './components/client/EditClient.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld, 
    EditClient,
  }
}
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
