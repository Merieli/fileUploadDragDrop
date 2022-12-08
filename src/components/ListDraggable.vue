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
        @drop="onDropVertical($event)"
      >
        <div
          v-for="(card, index) in itemsVertical.done"
          :key="card.id"
          class="card"
          ref="cardItems"
          draggable="true"
          @dragstart="startDragVertical($event, index)"
          @focus="bla()"
          @mouseenter="test(index)"
        >
          [{{ card.id }}] {{ card.title }}
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, Ref, ref } from 'vue';

export default defineComponent({
  setup() {
    const cardItems = ref([]);
    interface Item {
      id: number;
      title: string;
      list: number;
      dragging: boolean;
    }

    interface ItemVertical {
      id: number;
      title: string;
      dragging: boolean;
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

    const itemsVertical: Ref<ListVertical> = ref({
      done: [
        {
          id: 0,
          title: 'Item A',
          dragging: false,
        },
        {
          id: 1,
          title: 'Item B',
          dragging: false,
        },
        {
          id: 2,
          title: 'Item C',
          dragging: false,
        },
        {
          id: 3,
          title: 'Item D',
          dragging: false,
        },
        {
          id: 4,
          title: 'Item E',
          dragging: false,
        },
      ],
    });

    console.log(itemsVertical);

    const dropZoneA = ref(false);
    const dropZoneB = ref(false);

    const getList = (list: number): Item[] => items.value.filter((item) => item.list === list);

    interface EventDragTransfer {
      dataTransfer: {
        dropEffect: string;
        effectAllowed: string;
        target: any;
        setData(name: string, value: any): any;
        getData(name: string): any;
        preventDefault(): void;
      };
    }
    const getPosition = (index: number): any => {
      const cardHTML: HTMLElement = cardItems.value[index];
      const { top, bottom } = cardHTML.getBoundingClientRect();
      return {
        top,
        bottom,
      };
    };

    const startDrag = (event: EventDragTransfer, card: Item): void => {
      // Ativa o highlight das áreas dropáveis
      dropZoneA.value = true;
      dropZoneB.value = true;

      // eslint-disable-next-line
      card.dragging = true;
      // eslint-disable-next-line
      event.dataTransfer.effectAllowed = 'move';
      // eslint-disable-next-line
      event.dataTransfer.dropEffect = 'move';
      event.dataTransfer.setData('ItemID', card.id);
    };

    // onMounted(() => {
    //   //
    // });

    const startDragVertical = (event: EventDragTransfer, index: number): void => {
      const { top, bottom } = getPosition(index);
      console.log(top, bottom);

      // Ativa o highlight das áreas dropáveis
      dropZoneA.value = true;
      dropZoneB.value = true;

      itemsVertical.value.done[index].dragging = true;
      // eslint-disable-next-line
      event.dataTransfer.effectAllowed = 'move';
      // eslint-disable-next-line
      event.dataTransfer.dropEffect = 'move';
      event.dataTransfer.setData('ItemID', index);
      event.dataTransfer.setData('InitialTop', top);
      event.dataTransfer.setData('InitialBottom', bottom);
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

      console.log(items.value);

      // Remover classe na área que o card está sendo dropado
    };

    const onDropVertical = (event: DragEvent): void => {
      // Pegar o ID do item
      const itemIndex = event.dataTransfer?.getData('ItemID');
      const initialTop = event.dataTransfer?.getData('InitialTop');
      const initialBottom = event.dataTransfer?.getData('InitialBottom');

      // Atribuir a nova possição do item
      if (itemIndex) {
        const moveItem = itemsVertical.value.done[Number(itemIndex)];

        // remove o item da posição anterior
        itemsVertical.value.done.splice(Number(itemIndex), 1);

        // Validar a altura do mouse em que foi feito o drop
        const dropInHeigth = event.clientY;
        console.log(
          'Posição inicial >>>>>>>> de',
          initialTop,
          ' até ',
          initialBottom,
          'Mouse soltou em: ',
          dropInHeigth
        );

        // Criar uma função getIndexByPosition que itera sobre todos os items usando o ref e capturando suas posições
        /*
         na função fazer: 
         if(dropInHeigth >= initialTop && dropInHeigth <= (initialBottom - (initialBottom - initialTop) / 2) )
            //pegar o elemento um indice acima de onde o mouse parou
          
          if(dropInHeigth > (initialBottom - (initialBottom - initialTop) / 2) && dropInHeigth <= initialBottom )
            //pegar o elemento um indice abaixo de onde o mouse parou
          
         */

        // Adiciona o item a posicao desejada
        // Substituir o numero 2 abaixo pelo index de onde o mouse esta para soltar o
        itemsVertical.value.done.splice(2, 0, moveItem);
      }

      console.log('>>>>>', itemsVertical.value.done);

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
  width: 100%;
  min-height: 200px;
  padding: 20px;
  margin: 30px;
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
}

.highlight {
  background-color: #24c5152b;
}

.card {
  height: 50px;
  width: calc(100% - 20px);
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
