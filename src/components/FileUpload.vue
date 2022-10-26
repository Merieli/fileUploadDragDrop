<template>
  <div class="fileUpload">
    <input class="fileUpload__input"
      style="display:none"
      type="file"
      accept="image/*"
      id="file"
      aria-label="arquivo"
      ref="inputFile"
      :multiple="many"
      @change="handleInput"
    >
    <div class="fileUpload__clickZone"
      @click="$refs.inputFile.click"
    >
      Clique para fazer Upload
      <div class="fileUpload__dropZone"
        id="test"
        :style="dragging ? 'display: block' : 'display: none'"
        :class="entering ? 'fileUpload__dropZone--dropable': 'fileUpload__dropZone--' "
        @click.stop
        @dragenter="entering = true"
        @dragleave.prevent="entering = false"
        @drop.prevent="handleDrop"
        @dragover.prevent
      >
        Solte a imagem aqui
      </div>
    </div>
    <span v-if="file">{{ file.name }}</span>

  </div>
</template>

<script lang="ts">
import {
  defineComponent, ref, reactive,
} from 'vue';
import { TFile } from '../interfaces/TFile';

export default defineComponent({
  props: {
    /**
     * Define se o input irá permitir mais de um arquivo para upload.
     */
    many: {
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    const entering = ref(false);

    const dragging = ref(false);

    const file: TFile = reactive({
      selectedFiles: null,
      files: [],
      name: '',
    });

    // const emit = defineEmits<{(e: 'upload', files: File[]):void}>();

    function handleDrop(e: DragEvent): void {
      file.selectedFiles = e.dataTransfer?.files as FileList;
      console.log(Array.from(file.selectedFiles));
    }

    function handleInput(e: InputEvent) : void {
      file.selectedFiles = (e.target as HTMLInputElement).files as FileList;
      console.log(Array.from(file.selectedFiles)[0]);
      console.log(file.selectedFiles.length);

      if (props.many && file.selectedFiles.length > 1) {
        // for (let i = 0; i < file.selectedFiles.length; i++) {
        //   // file.files.push(file.selectedFiles[i]);
        // }
      }
    }

    // Feature: Ao arrastar o mouse na janela do navegador, exibe a div com a informação
    // de que ela é dropavel e ao arrastar sobre a div dropavel ela fica mais escura
    // https://stackoverflow.com/questions/3144881/how-do-i-detect-a-html5-drag-event-entering-and-leaving-the-window-like-gmail-d
    const test = document.querySelector('html');

    test?.addEventListener('move', (e) => {
      e.preventDefault();
      console.log('Move');
    });

    test?.addEventListener('dragend', (e) => {
      e.preventDefault();
      console.log('saindo do body');
    });

    window.addEventListener('dragenter', () => {
      console.log('dragging window');
      dragging.value = true;
    });

    window.addEventListener('dragleave', () => {
      console.log('dragleave window');
      // dragging.value = false;
    });

    // window.ondrag = (e) => console.log('dragging');

    return {
      file,
      entering,
      handleDrop,
      handleInput,
      dragging,
    };
  },
});
</script>

<style>
.fileUpload__clickZone{
  background-color: rgba(136, 136, 136, 0.26);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  height: 200px;
  width: 200px;
  position: relative;
}
.fileUpload__dropZone{
  background-color: rgb(210, 241, 241);
  border: 2px dashed #41b883;
  display: flex;
  cursor: grab;
  align-items: center;
  justify-content: center;
  height: 100%;
  width: 100%;
  /* visibility: hidden; */
  /* opacity: 0; */
  /* position: absolute; */
  top: 0;
  transition: .3s ease all;
}

.fileUpload__dropZone--dropable{
  display: block;
  background-color: rgb(162, 185, 185);
}
</style>
