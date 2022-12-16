<template>
  <button
    :class="`button ${color}`"
    :type="role"
    @click="$emit('click')"
    :disabled="disabled"
  >
    <slot></slot>
  </button>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import type { PropType, ButtonHTMLAttributes } from "vue";
export default defineComponent({
  emits: {
    click(): boolean {
      return true;
    },
  },
  props: {
    color: {
      type: String,
      default: "",
    },
    role: {
      type: String as PropType<ButtonHTMLAttributes["type"]>,
      default: "button",
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
});
</script>

<style lang="scss" scoped>
.button {
  font-weight: 400;
  font-size: 14px;
  line-height: 17px;
  font-family: "SFProDisplay";
  font-weight: 400;
  width: 100%;
  height: 39px;
  border-radius: 8px;
  background-color: #fff;
  color: #2d2d2d;
  &.red {
    background: #fa7272;
    color: #fff;
  }
  &.small {
    height: 33px;
  }
}
</style>
