<template>
  <div class="lottie-wrapper">
    <lottie
      id="savedanim"
      class="status"
      :options="savedData"
      :height="28"
      :width="28"
      v-on:animCreated="handleAnimation"
    />
  </div>
</template>

<script>
import Lottie from "vue-lottie";
import * as SavedAnimation from "../assets/saved.json";
export default {
  name: "Saved",
  components: {
    lottie: Lottie
  },
  data: function() {
    return {
      savedData: {
        animationData: SavedAnimation.default,
        loop: false
      }
    };
  },
  methods: {
    handleAnimation: function(anim) {
      this.anim = anim;
    },
    restart: function() {
      this.anim.goToAndPlay(0, true);
    }
  }
};
</script>
<style lang="scss" scoped>
.status {
  margin: 0 12px !important;
  opacity: 0.5;
}
</style>


