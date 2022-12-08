<template>
  <div>
    <sidebar-menu :show="show" />
    <button @click="open">Open Sidebar</button>
    <!-- <file-upload /> -->
    <list-draggable />
  </div>
</template>

<script lang="ts">
import SidebarMenu from '@/components/SidebarMenu.vue';
import { defineComponent } from 'vue';
// import FileUpload from './components/FileUpload.vue';
import ListDraggable from './components/ListDraggable.vue';

export default defineComponent({
  name: 'App',
  components: {
    // FileUpload,
    SidebarMenu,
    ListDraggable,
  },
  data() {
    return {
      show: false,
    };
  },
  methods: {
    open() {
      this.show = !this.show;
    },
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
