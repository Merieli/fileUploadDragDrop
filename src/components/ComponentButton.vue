<template>
  <div class="algo">
    <!--  eslint-disable-next-line -->
    <label for="teste">Toogle </label>
    <input
      type="checkbox"
      id="teste"
      name="teste"
      :checked="initialValue"
      @change="toogle"
    />
  </div>
  <button type="button" @click="clicked">{{ label }}</button>
</template>
<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    label: {
      type: String,
      required: true,
    },
    initialValue: {
      type: Boolean,
      required: true,
    },
  },
  emits: ['value', 'toggle'],
  setup(_, { emit }) {
    const clicked = (): void => {
      emit('value');
    };
    const toogle = (): void => {
      emit('toggle');
    };
    return {
      clicked,
      toogle,
    };
  },
});
</script>
<style lang="scss" scoped></style>
