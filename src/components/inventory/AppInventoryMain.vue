<template>
  <div class="main-inventory">
    <div
      class="main-inventory__cell"
      v-for="cell in dropCells"
      :key="cell.id"
      @drop="$emit('onDrop', $event, cell.id)"
      @dragover.prevent
      @dragenter.prevent
    >
      <div
        class="main-inventory__item"
        :class="{ active: activeItem?.id === item.id }"
        v-for="item in items.filter((item) => cell.id === item.dropCellId)"
        :key="item.id"
        draggable="true"
        @click.prevent="$emit('update:activeItem', item)"
        @dragstart="$emit('onDragStart', $event, item)"
      >
        <AppIcon :name="item.icon"></AppIcon>
        <span class="main-inventory__item-cnt">
          {{ item.cnt }}
        </span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, type PropType } from "vue";
import type { DropCell, DragItem } from "./inventory.types";
import AppIcon from "@/components/ui/AppIcon.vue";
export default defineComponent({
  components: {
    AppIcon,
  },
  props: {
    dropCells: {
      type: Array as PropType<DragItem[]>,
      default: () => [
        { id: 0 },
        { id: 1 },
        { id: 2 },
        { id: 3 },
        { id: 4 },
        { id: 5 },
        { id: 6 },
        { id: 7 },
        { id: 8 },
        { id: 9 },
        { id: 10 },
        { id: 11 },
        { id: 12 },
        { id: 13 },
        { id: 14 },
        { id: 15 },
        { id: 16 },
        { id: 17 },
        { id: 18 },
        { id: 19 },
        { id: 20 },
        { id: 21 },
        { id: 22 },
        { id: 23 },
        { id: 24 },
      ],
    },
    activeItem: {
      type: null as unknown as PropType<DragItem | null>,
      required: true,
      validator: (v: any) => typeof v === "object" || v === null,
    },
    items: {
      type: Array as PropType<DragItem[]>,
      default: () => [],
    },
  },
  setup() {
    return {};
  },
});
</script>

<style scoped lang="scss">
.main-inventory {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(5, 1fr);
  height: 500px;
  background-color: #4d4d4d;

  border: 1px solid #4d4d4d;
  width: 100%;
  border-radius: 12px;
  overflow: hidden;
  &__cell {
    border-left: 1px solid #4d4d4d;
    border-bottom: 1px solid #4d4d4d;
    background-color: #262626;
  }
  &__item {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    cursor: pointer;
    position: relative;
    transition: all 0.2s linear;
    &.active {
      background-color: #2f2f2f;
    }
    &-cnt {
      position: absolute;
      right: 0;
      bottom: -1px;
      font-weight: 500;
      font-size: 10px;
      line-height: 12px;
      border: 1px solid #4d4d4d;
      border-radius: 6px 0px 0px 0px;
      width: 16px;
      height: 16px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: rgba(#ffffff, 0.4);
    }
  }
}
</style>
