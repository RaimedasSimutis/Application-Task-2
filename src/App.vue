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
    /*background: url("https://ecoservice.lt/wp-content/uploads/2017/05/vilnius.jpg");*/
    background: #c4d3f6;
    padding: 0;
    margin: 0;
  }

  * {
    box-sizing: border-box;
    font-family: 'Poppins', sans-serif;
  }
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
}
</style>
