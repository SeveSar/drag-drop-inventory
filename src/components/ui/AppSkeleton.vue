<template>
  <div class="skeleton">
    <div class="skeleton__header">
      <div class="skeleton__img">
        <img src="@/assets/images/skeleton-img.png" alt="" />
      </div>
    </div>
    <div class="skeleton__body">
      <AppGhost
        v-for="item in ghostItems"
        :width="item.width"
        :height="item.height"
        :borderRadius="item.borderRadius"
        :key="item.id"
      ></AppGhost>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, type PropType } from "vue";
import AppGhost from "./AppGhost/AppGhost.vue";
import type { GhostItem } from "@/components/ui/AppGhost/ghost.types";

export default defineComponent({
  components: {
    AppGhost,
  },
  props: {
    ghostItems: {
      type: Array as PropType<GhostItem[]>,
      default: () => [
        {
          id: 0,
          width: (190 / 236) * 100 + "%",
          height: "30px",
          borderRadius: "8px",
        },
        {
          id: 0,
          width: (155 / 236) * 100 + "%",
          height: "10px",
          borderRadius: "4px",
        },
        {
          id: 1,
          width: (190 / 236) * 100 + "%",
          height: "10px",
          borderRadius: "4px",
        },
        {
          id: 2,
          width: (170 / 236) * 100 + "%",
          height: "10px",
          borderRadius: "4px",
        },
        {
          id: 3,
          width: (160 / 236) * 100 + "%",
          height: "10px",
          borderRadius: "4px",
        },
        {
          id: 4,
          width: (140 / 236) * 100 + "%",
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
  setup() {
    return {};
  },
});
</script>

<style scoped lang="scss">
.skeleton {
  background: #262626;
  border: 1px solid #4d4d4d;
  border-radius: 12px;
  height: 100%;
  padding-bottom: 24px;
  &__img {
    height: 240px;
    position: relative;
    img {
      position: absolute;
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
  &__header {
    padding: 18px 14px 20px 14px;
  }
  .ghost {
    margin: 0 auto;
    &:not(:last-child) {
      margin-bottom: 16px;
    }
    &:nth-child(6) {
      margin-bottom: 24px;
    }
    &:first-child {
      margin-bottom: 24px;
    }
  }
}
</style>
