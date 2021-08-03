<template>
  <div id="app">
    <Checkers/>
  </div>
</template>
<script>

  import Checkers from "@/components/Checkers";

export default {
  name: 'App',
  components: {
    Checkers

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
