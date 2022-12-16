<template>
  <div class="inventory">
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
          v-for="item in items.filter((item) => cell.id === item.dropCellId)"
          :key="item.id"
          draggable="true"
          @dragstart="onDragStart($event, item)"
        >
          <AppIcon :name="item.icon"></AppIcon>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import AppIcon from "@/components/ui/AppIcon.vue";

interface DropCell {
  id: number;
}
interface DragItem {
  id: number;
  dropCellId: number;
  icon: string;
}
export default defineComponent({
  components: {
    AppIcon,
  },
  setup() {
    // state
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
    const items = ref<DragItem[]>([
      { id: 0, dropCellId: 0, icon: "IconGreen" },
      { id: 1, dropCellId: 1, icon: "IconYellow" },
      { id: 2, dropCellId: 2, icon: "IconPurple" },
    ]);

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

      items.value = items.value.map((item) => {
        if (item.id === itemId) {
          return {
            ...item,
            dropCellId,
          };
        }
        return item;
      });
    };

    return {
      dropCells,
      items,
      onDrop,
      onDragStart,
      // onClick,
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
  }
}
</style>
