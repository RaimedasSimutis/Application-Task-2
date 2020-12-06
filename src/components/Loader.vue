<template>
  <transition name="fade">
    <div v-show="isActive" class="loader">
      <div class="loader__image"></div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Loader',
  props: {
    isActive: {
      type: Boolean,
      required: true,
    },
  },
};
</script>

<style scoped lang="scss">
  .loader {
    height: 100vh;
    width: 100vw;
    background-color: rgba(19, 18, 18, 0.56);
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1000;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .loader__image{
    height: 115px;
    width: 115px;
    border: 6px solid transparent;
    border-top-color: #0036f5;
    border-bottom-color: #00c5ff;
    border-radius: 50%;
    position: relative;
    -webkit-animation: spin 3s linear infinite;
    animation: spin 3s linear infinite;
  }

  .loader__image::before{
    content: "";
    position: absolute;
    top: 20px;
    right: 20px;
    bottom: 20px;
    left: 20px;
    border: 6px solid transparent;
    border-top-color: #6c6ff6;
    border-bottom-color: #6c6ff6;
    border-radius: 50%;
    -webkit-animation: spin 1.5s linear infinite;
    animation: spin 1.5s linear infinite;
  }

  @-webkit-keyframes spin {
    from {
      -webkit-transform: rotate(0deg);
      transform: rotate(0deg);
    }
    to {
      -webkit-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }

  @keyframes spin {
    from {
      -webkit-transform: rotate(0deg);
      transform: rotate(0deg);
    }
    to {
      -webkit-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }
</style>
