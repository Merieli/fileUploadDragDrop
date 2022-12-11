<template>
  <div class="boards">
    <div class="board">
      <h3>Todo</h3>
      <div
        class="dropzone"
        :class="{ highlight: dropZoneA }"
        @dragenter.prevent="dragEnter()"
        @dragover.prevent="dragOver()"
        @dragleave.prevent="dragLeave()"
        @drop="onDrop($event, 1)"
      >
        <div
          v-for="item in getList(1)"
          :key="item.id"
          class="card"
          :class="item.dragging === true ? 'is-dragging' : ''"
          draggable="true"
          @dragstart="startDrag($event, item)"
          @drag="drag()"
          @dragend="dragEnd()"
        >
          [{{ item.id }}] {{ item.title }}
        </div>
      </div>
    </div>
    <div class="board">
      <h3>In Progress</h3>
      <div
        class="dropzone"
        :class="{ highlight: dropZoneB }"
        @dragenter.prevent="dragEnter()"
        @dragover.prevent="dragOver()"
        @dragleave.prevent="dragLeave()"
        @drop="onDrop($event, 2)"
      >
        <div
          v-for="(item, index) in getList(2)"
          :key="index"
          class="card"
          draggable="true"
          @dragstart="startDrag($event, item)"
          @drag="drag()"
          @dragend="dragEnd()"
        >
          [{{ item.id }}] {{ item.title }}
        </div>
      </div>
    </div>

    <div class="board">
      <h3>Done</h3>
      <div
        class="dropzone"
        :class="{ highlight: dropZoneB }"
        @dragenter.prevent="dragEnter()"
        @dragover.prevent="dragOver()"
        @dragleave.prevent="dragLeave()"
        @drop.prevent="onDropVertical($event)"
      >
        <div
          v-for="(card, index) in itemsVertical.done"
          :key="card.id"
          class="card"
          :class="card.dragging === true ? 'is-dragging' : ''"
          ref="cardItems"
          draggable="true"
          @dragstart="startDragVertical($event, index)"
          @drag="drag()"
          @dragend="dragEnd()"
        >
          [{{ card.id }}] {{ card.title }}
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
// eslint-disable-next-line
import { computed, defineComponent, onMounted, Ref, ref } from 'vue';

export default defineComponent({
  setup() {
    const cardItems = ref([]);
    interface Item {
      id: number;
      title: string;
      list: number;
      dragging: boolean;
    }

    interface PositionsElement {
      top: number;
      bottom: number;
    }

    interface ItemVertical {
      id: number;
      title: string;
      dragging: boolean;
      position?: PositionsElement;
    }
    interface ListVertical {
      done: ItemVertical[];
    }

    const items: Ref<Item[]> = ref([
      {
        id: 0,
        title: 'Item A',
        list: 1,
        dragging: false,
      },
      {
        id: 1,
        title: 'Item B',
        list: 1,
        dragging: false,
      },
      {
        id: 2,
        title: 'Item C',
        list: 1,
        dragging: false,
      },
      {
        id: 3,
        title: 'Item D',
        list: 1,
        dragging: false,
      },
      {
        id: 4,
        title: 'Item E',
        list: 1,
        dragging: false,
      },
      {
        id: 5,
        title: 'Item F',
        list: 2,
        dragging: false,
      },
      {
        id: 6,
        title: 'Item G',
        list: 2,
        dragging: false,
      },
      {
        id: 7,
        title: 'Item H',
        list: 3,
        dragging: false,
      },
      {
        id: 8,
        title: 'Item I',
        list: 3,
        dragging: false,
      },
    ]);

    const getPosition = (index: number): any => {
      const cardHTML: HTMLElement = cardItems.value[index];
      const { top, bottom } = cardHTML.getBoundingClientRect();
      return {
        top,
        bottom,
      };
    };

    const itemsVertical: Ref<ListVertical> = ref({
      done: [
        {
          id: 0,
          title: 'Item A',
          dragging: false,
          position: computed(() => getPosition(0)),
        },
        {
          id: 1,
          title: 'Item B',
          dragging: false,
          position: computed(() => getPosition(1)),
        },
        {
          id: 2,
          title: 'Item C',
          dragging: false,
          position: computed(() => getPosition(2)),
        },
        {
          id: 3,
          title: 'Item D',
          dragging: false,
          position: computed(() => getPosition(3)),
        },
        {
          id: 4,
          title: 'Item E',
          dragging: false,
          position: computed(() => getPosition(4)),
        },
      ],
    });

    const dropZoneA = ref(false);
    const dropZoneB = ref(false);

    // eslint-disable-next-line
    const getList = (list: number): Item[] =>
      // eslint-disable-next-line
      items.value.filter((item) => item.list === list);

    const startDrag = (event: DragEvent, card: Item): void => {
      // Ativa o highlight das áreas dropáveis
      dropZoneA.value = true;
      dropZoneB.value = true;

      // eslint-disable-next-line
      card.dragging = true;
      // eslint-disable-next-line
      event.dataTransfer.effectAllowed = 'move';
      // eslint-disable-next-line
      event.dataTransfer.dropEffect = 'move';
      event.dataTransfer.setData('ItemID', card.id.toString());
    };

    onMounted(() => {
      // Atribuir a position como o computed após ser montado
      // Foreach que captura o indice de cada item e atribui a função computed
      // para calculo do position
    });

    const startDragVertical = (event: DragEvent, index: number): void => {
      // Ativa o highlight das áreas dropáveis
      dropZoneA.value = true;
      dropZoneB.value = true;

      itemsVertical.value.done[index].dragging = true;
      // eslint-disable-next-line
      event.dataTransfer.effectAllowed = 'move';
      // eslint-disable-next-line
      event.dataTransfer.dropEffect = 'move';
      event.dataTransfer.setData('ItemID', index.toString());

      // ghostImage
      const cloneGhost = cardItems.value[index].cloneNode(true);
      cloneGhost.style.background = 'white';
      cloneGhost.style.width = '200px';
      cloneGhost.style.backgroundImage = 'none';
      cloneGhost.style.pointerEvents = 'none';
      document.body.appendChild(cloneGhost);
      event.dataTransfer.setDragImage(cloneGhost, 0, 0);

      setTimeout(() => {
        document.body.removeChild(cloneGhost);
      });
    };

    const drag = (): void => {
      // console.log('> CARD: Is draggind');
    };

    const dragEnd = (): void => {
      // console.log('> CARD: Stop draggind');

      dropZoneA.value = false;
      dropZoneB.value = false;
      /* eslint-disable */
      items.value.forEach((card) => (card.dragging = false));

      itemsVertical.value.done.forEach((card) => (card.dragging = false));
    };

    // dropzone
    const dragEnter = (): void => {
      console.log('> DROPZONE: Enter in zone');
    };

    const dragOver = (): void => {
      console.log('> DROPZONE: In zone');
      // Adicionar classe a área que o card está sendo dropado

      // Filtrar o card que está sendo arrastado
      const cardBeingDragged = items.value.filter((card) => card.dragging === true);
      console.log(cardBeingDragged);
    };

    const dragLeave = (): void => {
      console.log('> DROPZONE: Leave');
      // Remover classe a área que o card está sendo dropado
    };

    const onDrop = (event: DragEvent, list: number): void => {
      // Pegar o ID do item
      const itemID = event.dataTransfer?.getData('ItemID');

      // Atribuir a nova possição do item
      if (itemID) items.value[Number(itemID)].list = list;

      // Remover classe na área que o card está sendo dropado
    };

    type IndexByPosition = { index: number; lastHalf: number };

    const getIndexByPosition = (dropInHeigth: number): IndexByPosition => {
      const indexInDroppedPosition: IndexByPosition = itemsVertical.value.done.reduce(
        (
          accumulator: IndexByPosition,
          element: ItemVertical,
          indexCurrent: number
        ): IndexByPosition => {
          const initialTopElement = element.position?.top;
          const initialBottomElement = element.position?.bottom;
          const heightOfCard = initialBottomElement - initialTopElement;
          const halfOfCard = heightOfCard / 2;
          const halfUp = initialTopElement + halfOfCard;

          const currrentIsClosest =
            Math.abs(halfUp - dropInHeigth) <
            Math.abs(accumulator.lastHalf - dropInHeigth)
              ? true
              : false;
          console.log(
            element.title,
            dropInHeigth,
            initialBottomElement,
            initialTopElement
          );
          if (dropInHeigth <= initialBottomElement && dropInHeigth >= initialTopElement) {
            return {
              index: indexCurrent,
              lastHalf: halfUp,
            };
          }

          // if (dropInHeigth > halfUp && (currrentIsClosest || accumulator.lastHalf == 0)) {
          //   console.log(
          //     'Index [',
          //     indexCurrent,
          //     '] Foi solto abaixo da metade do elemento, na altura: ',
          //     dropInHeigth,
          //     'em que a metade do elemento é: ',
          //     halfUp,
          //     'e a metade do ultimo elemento acumulado é: ',
          //     accumulator.lastHalf
          //   );

          //   return {
          //     index: indexCurrent + 1,
          //     lastHalf: halfUp,
          //   };
          // }
          // if (dropInHeigth < halfUp && (currrentIsClosest || accumulator.lastHalf == 0)) {
          //   console.log(
          //     'Index [',
          //     indexCurrent,
          //     '] Foi solto acima da metade no elemento, na altura: ',
          //     dropInHeigth,
          //     'em que a metade do elemento é: ',
          //     halfUp,
          //     'e a metade do ultimo elemento acumulado é: ',
          //     accumulator.lastHalf
          //   );
          //   return {
          //     index: indexCurrent,
          //     lastHalf: halfUp,
          //   };
          // }

          return {
            index: accumulator.index,
            lastHalf: halfUp,
          };
        },
        {
          index: 0,
          lastHalf: 0,
        }
      );
      return indexInDroppedPosition;
    };

    const onDropVertical = (event: DragEvent): void => {
      // Pegar o ID do item
      const itemIndex = Number(event.dataTransfer?.getData('ItemID'));

      // Atribuir a nova possição do item
      if (itemIndex) {
        const moveItem = itemsVertical.value.done[itemIndex];

        // remove o item da posição anterior
        itemsVertical.value.done.splice(itemIndex, 1);

        // Validar a altura do mouse em que foi feito o drop
        const dropInHeigth = event.clientY;
        const moveTo: IndexByPosition = getIndexByPosition(dropInHeigth);

        // Adiciona o item a posicao desejada
        itemsVertical.value.done.splice(moveTo.index, 0, moveItem);

        return;
      }

      // Remover classe a área que o card está sendo dropado
    };

    return {
      items,
      getList,
      startDrag,
      startDragVertical,
      drag,
      dragEnd,
      onDrop,
      onDropVertical,
      dragEnter,
      dragOver,
      dragLeave,
      dropZoneA,
      dropZoneB,
      itemsVertical,
      cardItems,
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
</style>
