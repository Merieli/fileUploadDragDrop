<template>
  <div class="col-6 board">
    <h3>Library Vue Draggable {{ draggingInfo }}</h3>

    <draggable
      :list="list"
      :disabled="!enabled"
      item-key="name"
      class="list-group dropzone"
      ghost-class="ghost"
      :move="checkMove"
      @start="dragging = true"
      @end="dragging = false"
    >
      <template #item="{ element }">
        <div class="list-group-item card" :class="{ 'not-draggable': !enabled }">
          [{{ element.id }}] {{ element.name }}
        </div>
      </template>
    </draggable>
  </div>
</template>
<script lang="ts">
import { computed, defineComponent, ref } from 'vue';
import draggable from 'vuedraggable';

export default defineComponent({
  components: {
    draggable,
  },
  setup() {
    const list = ref([
      { name: 'John', text: '', id: 0 },
      { name: 'Joao', text: '', id: 1 },
      { name: 'Jean', text: '', id: 2 },
      { name: 'Merieli', text: '', id: 3 },
      { name: 'Welington', text: '', id: 4 },
    ]);
    const enabled = ref(true);
    const dragging = ref(false);

    const draggingInfo = computed(() => (dragging.value ? 'under drag' : ''));

    const checkMove = (e: any): void => {
      window.console.log(`Future index: ${e.draggedContext.futureIndex}`);
      console.log(list.value);
    };

    return {
      list,
      enabled,
      dragging,
      draggingInfo,
      checkMove,
    };
  },
});
</script>

<style scoped>
.boards {
  display: flex;
  justify-content: space-between;
  background-color: rgb(231, 240, 180);
  min-height: 200px;
  padding: 20px;
  margin: 30px;
  max-width: 100vw;
}

.board {
  background-color: #868780;
  margin: 10px;
  width: 340px;
  min-height: 10px;
}

.dropzone {
  min-width: 332px;
  min-height: 200px;
  height: auto;
  display: block;
  z-index: 2;
  padding: 10px 0;
}

.highlight {
  background-color: #24c5152b;
}

.card {
  height: 50px;
  width: calc(100% - 20px);
  cursor: move;
  background-color: white;
  margin: 10px;
}

.card,
.dropzone {
  transition: 400ms;
}

.is-dragging {
  cursor: move;
  opacity: 0.2;
}

.buttons {
  margin-top: 35px;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.not-draggable {
  cursor: no-drop;
}
</style>
