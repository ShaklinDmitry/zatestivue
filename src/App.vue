<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <ShowStatement msg="Это приложение для цитат"/>
  </div>
</template>

<script>
import ShowStatement from "@/components/ShowStatement";

export default {
  name: 'App',
  components: {
    ShowStatement,
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
