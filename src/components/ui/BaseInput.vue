<template>
  <div class="form-group">
    <input
      :class="['form-control', { error: error }]"
      :value="modelValue"
      @input="updateValue"
      v-bind="$attrs"
      @focus="$emit('onFocus')"
    />
    <transition name="fade">
      <small v-if="error">{{ error }}</small>
    </transition>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  inheritAttrs: false,

  props: {
    modelValue: {
      type: [String, Number],
      default: "",
    },
    id: {
      type: String,
      default: "",
    },
    error: {
      type: String,
      default: "",
    },
  },
  emits: ["update:modelValue", "onFocus"],
  setup(props, ctx) {
    const updateValue = (e: Event) => {
      ctx.emit("update:modelValue", (e.target as HTMLInputElement).value);
    };
    return {
      updateValue,
    };
  },
});
</script>

<style scoped lang="scss">
.form-group {
  position: relative;
}
.form-control {
  font-weight: 500;
  font-size: 14px;
  line-height: 17px;
  background: #262626;
  border: 1px solid #4d4d4d;
  border-radius: 4px;
  color: #fff;
  height: 40px;
  width: 100%;
  padding: 0 12px;
  &::placeholder {
    color: rgba(#fff, 0.4);
  }
}

small {
  color: red;
  display: inline-block;
  font-size: 15px;
  position: absolute;
  bottom: -20px;
  z-index: 1;
  left: 0;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
