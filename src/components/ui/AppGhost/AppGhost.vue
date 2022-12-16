<template>
  <div class="ghost" :style="styles"></div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    width: {
      type: String,
      default: "100%",
    },
    height: {
      type: String,
      default: "100%",
    },
    borderRadius: {
      type: String,
      default: "10px",
    },
  },
  computed: {
    styles() {
      return {
        width: this.width,
        height: this.height,
        borderRadius: this.borderRadius,
      };
    },
  },
});
</script>

<style scoped lang="scss">
:root {
  --loading-grey: #ededed;
}
.ghost {
  background-color: #ededed;
  background: linear-gradient(90deg, #3c3c3c 0%, #444444 51.04%, #333333 100%);
  background-size: 200% 100%;
  background-position-x: 180%;
  animation: 1s loading ease-in-out infinite;
  @keyframes loading {
    to {
      background-position-x: -20%;
    }
  }
}
</style>
