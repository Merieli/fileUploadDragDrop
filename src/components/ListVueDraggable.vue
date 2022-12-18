<template>
  <div class="col-6 board">
    <h3>Library Vue Draggable {{ draggingInfo }}</h3>

    <draggable
      :list="list02.lista"
      :disabled="!activeDrag"
      class="dropzone"
      ghost-class="ghost"
      :move="checkMove"
      @start="isDragging = true"
      @end="isDragging = false"
      item-key="id"
    >
      <template #item="{ element, index }">
        <div class="list-group-item card" :class="{ 'not-draggable': !activeDrag }">
          <ComponentButton
            class="teste"
            :label="element.name"
            :initial-value="element.active"
            @toggle="getInfo(element.id)"
            @value="getInfo(index)"
          />
        </div>
      </template>
    </draggable>
  </div>
</template>
<script lang="ts">
// eslint-disable-next-line
import { computed, defineComponent, reactive, ref } from 'vue';
import draggable from 'vuedraggable';
import ComponentButton from './ComponentButton.vue';

export default defineComponent({
  components: {
    draggable,
    ComponentButton,
  },
  setup() {
    const list = ref([
      {
        name: 'John',
        text: '',
        id: 1,
        active: true,
      },
      {
        name: 'Joao',
        text: '',
        id: 2,
        active: true,
      },
      {
        name: 'Jean',
        text: '',
        id: 3,
        active: true,
      },
      {
        name: 'Merieli',
        text: '',
        id: 4,
        active: true,
      },
      {
        name: 'Welington',
        text: '',
        id: 5,
        active: true,
      },
    ]);

    const list02 = reactive({
      lista: [],
    });
    const activeDrag = ref(true);
    const isDragging = ref(false);

    list02.lista.push({
      name: 'John',
      text: '',
      id: 1,
      active: true,
    });

    list02.lista.push({
      name: 'Joao',
      text: '',
      id: 2,
      active: true,
    });

    const draggingInfo = computed(() => (isDragging.value ? 'under drag' : ''));

    const checkMove = (e: any): void => {
      window.console.log(`Future index: ${e.draggedContext.futureIndex}`);
      console.log(list.value);
    };

    const getInfo = (id: number): void => {
      console.log('O id: ', id);
    };

    return {
      list,
      list02,
      activeDrag,
      isDragging,
      draggingInfo,
      checkMove,
      getInfo,
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
