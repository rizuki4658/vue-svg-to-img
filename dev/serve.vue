<script lang="ts">
import { defineComponent } from 'vue';
import VueSvgToImg from '@/vue-svg-to-img.vue';

export default defineComponent({
  name: 'ServeDev',
  components: {
    VueSvgToImg
  }
});
</script>

<template>
  <div id="app">
    <VueSvgToImg
      src="icons/bubble.svg"
      color="#800080"
      color-hover="red"
      :static="false"
      :width="320"
      :height="320"
    />
    <VueSvgToImg
      src="icons/bubble.svg"
      color="red"
      :width="320"
      :height="320"
    />
  </div>
</template>
