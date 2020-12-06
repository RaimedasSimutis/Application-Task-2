<template>
  <div id="app">
    <MainView/>
  </div>
</template>

<script>
import MainView from './components/MainView.vue';

export default {
  name: 'App',
  components: {
    MainView,
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
