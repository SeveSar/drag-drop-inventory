<template>
  <div class="inventory">
    <div class="inventory__aside">
      <AppModal
        :currentItem="activeItem"
        @close="activeItem = null"
        @onSumbit="changeCnt"
      >
        <template #header>
          <AppIcon v-if="activeItem" :name="activeItem.icon"></AppIcon>
        </template>
        <template #footer>
          <AppIcon v-if="activeItem" :name="activeItem.icon"></AppIcon>
        </template>
      </AppModal>
    </div>

    <div class="inventory__main">
      <AppInventoryMain
        @update:activeItem="setActiveItem"
        :activeItem="activeItem"
        :items="items"
        @onDrop="onDrop"
        @onDragStart="onDragStart"
      />
      <AppModal
        :currentItem="activeItem"
        @close="activeItem = null"
        @onSumbit="changeCnt"
      />
    </div>
    <div class="inventory__footer">
      <AppIcon class="inventory__close" name="IconClose"></AppIcon>
      <AppGhost height="36px" borderRadius="12px"></AppGhost>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, nextTick } from "vue";
import AppIcon from "@/components/ui/AppIcon.vue";
import AppModal from "./AppModal.vue";
import type { DropCell, DragItem } from "./inventory.types";
import AppGhost from "../ui/AppGhost.vue";
import AppInventoryMain from "./AppInventoryMain.vue";
export default defineComponent({
  components: {
    AppIcon,
    AppModal,
    AppInventoryMain,
    AppGhost,
  },
  setup() {
    // state
    const activeItem = ref<DragItem | null>(null);

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
      console.log(event);
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
    const removeItem = (id: number) => {
      items.value = items.value.filter((item) => item.id !== id);
      const isEmptyInventory = items.value.every((item) => !item.cnt);
      if (isEmptyInventory) {
        localStorage.clear();
      }
    };
    const changeCnt = (cnt: number) => {
      items.value = items.value.map((item) => {
        if (activeItem.value?.id === item.id) {
          return {
            ...item,
            cnt: Math.max(0, item.cnt - cnt),
          };
        }
        return item;
      });
      activeItem.value = null;
      localStorage.setItem("ITEMS", JSON.stringify(items.value));
      items.value.forEach((item) => {
        if (item.cnt <= 0) {
          removeItem(item.id);
        }
      });
    };

    return {
      items,
      onDrop,
      onDragStart,
      setActiveItem,
      activeItem,
      changeCnt,
    };
  },
});
</script>

<style scoped lang="scss">
.inventory {
  display: grid;
  justify-content: center;
  padding: 25px;
  grid-template-columns: 260px 525px;
  grid-template-rows: auto auto;
  max-width: 784px;

  width: 100%;
  margin: 0 auto;

  &__main {
    width: 100%;
    grid-column-start: span 3;
    position: relative;
    overflow: hidden;
  }
  &__aside {
    grid-column-start: span 1;
    margin-right: 24px;
  }
  &__footer {
    margin-top: 24px;
    background: #262626;
    grid-column-start: span 5;
    padding: 18px 66px 18px 18px;
    border: 1px solid #4d4d4d;
    border-radius: 12px;
    position: relative;
  }
  &__close {
    position: absolute;
    top: 14px;
    right: 14px;
  }
}
</style>
