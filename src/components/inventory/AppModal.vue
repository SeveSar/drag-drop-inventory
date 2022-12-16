<template>
  <div class="overlay" v-if="currentItem" @click.self="close"></div>
  <transition name="slide">
    <div v-if="currentItem" class="modal-inventory">
      <button class="modal-inventory__close" @click="close">
        <AppIcon name="IconClose"></AppIcon>
      </button>
      <div class="modal-inventory__header">
        <AppIcon :name="currentItem.icon"></AppIcon>
      </div>

      <div class="modal-inventory__body">
        <AppGhost
          v-for="item in ghostItems"
          :key="item.id"
          :width="item.width"
          :height="item.height"
          :borderRadius="item.borderRadius"
        ></AppGhost>
      </div>
      <div class="modal-inventory__footer">
        <BaseButton
          v-if="!isControlsActive"
          @click="isControlsActive = true"
          color="red"
        >
          Удалить предмет
        </BaseButton>
        <div v-else class="modal-inventory__controls">
          <form @submit.prevent="onSubmitForm">
            <BaseInput
              placeholder="Введите количество"
              v-model="inputCnt"
              type="number"
              :max="currentItem.cnt"
            ></BaseInput>
            <div class="modal-inventory__actions">
              <BaseButton
                color="small"
                @click="isControlsActive = false"
                role="button"
              >
                Отмена
              </BaseButton>
              <BaseButton color="small red" role="submit">
                Подтвердить
              </BaseButton>
            </div>
          </form>
        </div>
      </div>
    </div>
  </transition>
</template>

<script lang="ts">
import { defineComponent, type PropType, ref } from "vue";
import AppIcon from "../ui/AppIcon.vue";
import BaseInput from "../ui/BaseInput.vue";
import BaseButton from "../ui/BaseButton.vue";
import type { DragItem } from "./inventory.types";
import AppGhost from "@/components/ui/AppGhost/AppGhost.vue";
import type { GhostItem } from "@/components/ui/AppGhost/ghost.types";
export default defineComponent({
  components: {
    AppIcon,
    AppGhost,
    BaseButton,
    BaseInput,
  },
  emits: ["close", "onSumbit"],
  props: {
    currentItem: {
      type: null as unknown as PropType<DragItem | null>,
      required: true,
      validator: (v: any) => typeof v === "object" || v === null,
    },
    ghostItems: {
      type: Array as PropType<GhostItem[]>,
      default: () => [
        {
          id: 0,
          width: "100%",
          height: "30px",
          borderRadius: "8px",
        },
        {
          id: 1,
          width: "100%",
          height: "10px",
          borderRadius: "4px",
        },
        {
          id: 2,
          width: "100%",
          height: "10px",
          borderRadius: "4px",
        },
        {
          id: 3,
          width: "100%",
          height: "10px",
          borderRadius: "4px",
        },
        {
          id: 4,
          width: (180 / 236) * 100 + "%",
          height: "10px",
          borderRadius: "4px",
        },
        {
          id: 5,
          width: (80 / 236) * 100 + "%",
          height: "10px",
          borderRadius: "4px",
        },
      ],
    },
  },

  setup(props, { emit }) {
    const isControlsActive = ref(false);
    const inputCnt = ref();

    const onSubmitForm = () => {
      if (!inputCnt.value) {
        alert("Введите кол-во");
        return;
      }
      emit("onSumbit", inputCnt.value);
      inputCnt.value = "";
      isControlsActive.value = false;
    };
    const close = () => {
      emit("close");
      isControlsActive.value = false;
    };
    return {
      isControlsActive,
      onSubmitForm,
      inputCnt,
      close,
    };
  },
});
</script>

<style scoped lang="scss">
.overlay {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 555;
}
.modal-inventory {
  z-index: 999;
  max-width: 250px;
  width: 100%;
  position: absolute;
  top: 0;
  right: 0;
  height: 100%;
  border: 1px solid #4d4d4d;
  backdrop-filter: blur(8px);
  background: rgba(38, 38, 38, 0.5);
  border-radius: 0 12px 12px 0px;
  overflow: hidden;
  &__header {
    position: relative;
    padding: 55px 30px 30px;
    display: flex;
    svg {
      width: 130px;
      height: 130px;
      margin: auto;
    }
  }
  &__close {
    position: absolute;
    top: 14px;
    right: 14px;
    z-index: 1;
  }
  &__body {
    padding: 16px 20px 24px;
    position: relative;

    &::after {
      content: "";
      height: 1px;
      width: 88%;
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      background-color: #4d4d4d;
    }
    &::after {
      top: 0;
    }
  }
  &__footer {
    margin-top: 24px;
    padding: 0 15px;
    position: absolute;
    bottom: 0;
    padding-bottom: 20px;
    width: 100%;
    padding-top: 20px;
    background-color: #262626;
    border-top: 1px solid #4d4d4d;
  }
  &__actions {
    display: flex;
    margin-top: 20px;
    .button {
      width: auto;
      padding: 0 18px;
      flex-grow: 1;
      & + .button {
        margin-left: 10px;
      }
    }
  }
  .ghost {
    margin: auto;
    &:not(:last-child) {
      margin-bottom: 16px;
    }
    &:first-child {
      margin-bottom: 24px;
    }
  }
}
.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s ease;
}

.slide-enter-from,
.slide-leave-to {
  transform: translateX(100%);
}
</style>
