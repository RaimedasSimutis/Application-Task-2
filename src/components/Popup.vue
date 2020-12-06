<template>
  <transition name="slide-fade">
  <div v-show="isPopupActive" class="popup">
    <a class="popup__button-close" @click="closePopup">
      <img src="../assets/times-solid.svg" alt="close">
    </a>
    <h3 class="popup__heading">Info</h3>
    <p class="popup_paragraph">Last clicked cell data: {{message}}</p>
  </div>
  </transition>
</template>

<script>
export default {
  name: 'Popup',
  props: {
    duration: {
      type: Number,
      default: 3000,
    },
  },
  data() {
    return {
      isPopupActive: false,
      message: null,
      timeout: null,
    };
  },
  methods: {
    openPopup(message) {
      if (message) {
        this.message = message;
        this.isPopupActive = true;

        if (this.timeout) {
          clearTimeout(this.timeout);
        }

        this.timeout = setTimeout(() => {
          this.closePopup();
        }, this.duration);
      }
    },
    closePopup() {
      this.isPopupActive = false;
    },
  },
};
</script>

<style scoped lang="scss">
  .popup {
    position: fixed;
    bottom: 10px;
    right: 10px;
    width: 300px;
    background-color: #6c7ae0;
    color: #ffffff;
    border-radius: 10px;
    padding: 15px;

    &__heading {
      margin: 0;
    }

    &__button-close {
      position: absolute;
      top: 10px;
      right: 10px;
      cursor: pointer;

      img {
        height: 20px;
        width: 20px;
      }
    }
  }
</style>
