<template>
    <transition name="slide-sidebar">
        <aside v-if="show" class="sidebar">
            <h1>Title of Sidebar</h1>
            <p>lang1</p>
            <p>lang1</p>
            <p>lang1</p>
            <p>lang1</p>
            <p>lang1</p>
            <p>lang1</p>
            <p>lang1</p>
            <p>lang1</p>
            <p>lang1</p>
        </aside>
    </transition>
</template>
<script lang='ts'>
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    show: {
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    return {

    };
  },
});
</script>
<style scoped>
.sidebar{
    position: absolute;
    max-width: 360px;
    background-color: white;
    box-shadow: 1px 1px 11px 0px #00000036;
    z-index: 10;
    left: 0;
}

.slide-sidebar-leave-active,
.slide-sidebar-enter-active{
 transition: all 0.8s ease-out 0.05s;
 /* transition: transform 0.8s cubic-bezier(0.250, 0.460, 0.450, 0.940) 0.4s both; */
}

.slide-sidebar-enter-from,
.slide-sidebar-leave-to {
  transform: translateX(-360px);
  opacity: 0;
}
</style>
