<template>
  <div id="app">
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
};
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');
  @import "./styles/animations.scss";

  html, body {
    background: #c4d3f6;
    padding: 0;
    margin: 0;
  }

  * {
    box-sizing: border-box;
    font-family: 'Poppins', sans-serif;
  }
</style>
