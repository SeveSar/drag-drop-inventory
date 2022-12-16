<template>
  <div class="inventory">
    <div class="inventory__main">
      <div class="inventory__grid">
        <div
          class="inventory__cell"
          v-for="cell in dropCells"
          :key="cell.id"
          @drop="onDrop($event, cell.id)"
          @dragover.prevent
          @dragenter.prevent
        >
          <div
            class="inventory__item"
            :class="{ active: activeItem?.id === item.id }"
            v-for="item in items.filter((item) => cell.id === item.dropCellId)"
            :key="item.id"
            draggable="true"
            @click.prevent="setActiveItem(item)"
            @dragstart="onDragStart($event, item)"
          >
            <AppIcon :name="item.icon"></AppIcon>
            <span class="inventory__item-cnt">
              {{ item.cnt }}
            </span>
          </div>
        </div>
      </div>
      <AppMoodal
        :currentItem="activeItem"
        @close="activeItem = null"
        @onSumbit="changeCnt"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, nextTick } from "vue";
import AppIcon from "@/components/ui/AppIcon.vue";
import AppMoodal from "./AppMoodal.vue";
import type { DropCell, DragItem } from "./inventory.types";
export default defineComponent({
  components: {
    AppIcon,
    AppMoodal,
  },
  setup() {
    // state
    const activeItem = ref<DragItem | null>(null);
    const dropCells: DropCell[] = [
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
    ];
    const savedItems = localStorage.getItem("ITEMS");
    const parsedItems = savedItems ? JSON.parse(savedItems) : savedItems;

    const items = ref<DragItem[]>(
      parsedItems || [
        { id: 0, dropCellId: 0, icon: "IconGreen", cnt: 4 },
        { id: 1, dropCellId: 1, icon: "IconYellow", cnt: 2 },
        { id: 2, dropCellId: 2, icon: "IconPurple", cnt: 5 },
      ]
    );

    //methods
    const onDragStart = (event: DragEvent, item: any) => {
      if (event.dataTransfer?.effectAllowed) {
        event.dataTransfer.effectAllowed = "move";
      }
      if (event.dataTransfer?.dropEffect) {
        event.dataTransfer.dropEffect = "move";
      }
      event.dataTransfer?.setData("itemId", item.id.toString());
    };

    const onDrop = (event: DragEvent, dropCellId: number) => {
      const itemId = Number(event.dataTransfer?.getData("itemId"));
      const itemTheSameCellId = items.value.find(
        (item) => item.dropCellId === dropCellId
      );
      const currentItem = items.value.find((item) => item.id === itemId);
      if (itemTheSameCellId && currentItem) {
        itemTheSameCellId.dropCellId = currentItem.dropCellId;
      }
      if (currentItem) {
        currentItem.dropCellId = dropCellId;
      }
      localStorage.setItem("ITEMS", JSON.stringify(items.value));
    };
    const setActiveItem = (item: DragItem) => {
      if (item.id !== activeItem.value?.id) {
        activeItem.value = null;
        nextTick(() => {
          activeItem.value = item;
        });
      }
    };

    return {
      dropCells,
      items,
      onDrop,
      onDragStart,
      setActiveItem,
      activeItem,
    };
  },
});
</script>

<style scoped lang="scss">
.inventory {
  background: #262626;
  display: flex;
  justify-content: center;
  padding: 25px;

  &__main {
    max-width: 525px;
    width: 100%;
    position: relative;
    overflow: hidden;
  }
  &__grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(5, 1fr);
    height: 500px;
    background-color: #4d4d4d;
    max-width: 525px;
    border: 1px solid #4d4d4d;
    width: 100%;
    border-radius: 12px;
    overflow: hidden;
  }
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
