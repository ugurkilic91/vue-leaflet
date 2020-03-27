<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Example />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Example from './components/deneme.vue'

export default {
  name: 'App',
  components: {

    Example
  }
}
</script>

<style>
body{ padding:0px;margin: 0px;}
</style>