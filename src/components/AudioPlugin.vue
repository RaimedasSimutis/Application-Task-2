<template>
  <div class="audio-plugin">
    <a v-if="isAudioPlaying" @click="stopMusic">
      <img
        class="audio-plugin__icon"
        src="../assets/volume-up-solid.svg"
        alt="audio"
      >
    </a>
    <a v-else @click="playBackgroundMusic">
      <img
        class="audio-plugin__icon"
        src="../assets/volume-mute-solid.svg"
       >
    </a>

    <span>{{trackName}}</span>
  </div>
</template>

<script>

export default {
  name: 'AudioPlugin',
  props: {
    trackName: {
      type: String,
      required: true,
    },
    trackPath: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      backgroundAudio: null,
      isAudioPlaying: false,
    };
  },
  computed: {
    importAudio() {
      return require('../assets/komunalininku-balius.mp3');
    },
  },
  created() {
    this.backgroundAudio = new Audio(this.importAudio);
    this.backgroundAudio.addEventListener('ended', () => {
      this.isAudioPlaying = false;
    });
  },
  methods: {
    playBackgroundMusic() {
      if (this.backgroundAudio) {
        this.backgroundAudio.load();
        this.backgroundAudio.play();
        this.isAudioPlaying = true;
      }
    },
    stopMusic() {
      if (this.backgroundAudio) {
        this.backgroundAudio.pause();
        this.isAudioPlaying = false;
      }
    },
  },
};
</script>

<style scoped lang="scss">
  .audio-plugin {
    position: fixed;
    bottom: 15px;
    left: 15px;

    &__icon {
      height: 14px;
      margin-right: 5px;

      &:hover {
        cursor: pointer;
      }
    }
  }
</style>
