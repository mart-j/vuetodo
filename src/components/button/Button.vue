<template>
  <button @click="$emit('clickHandler')" class="button" :type="type">
    <img :src="label" width="25" />
  </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Button',
  props: {
    label: { type: String, required: true },
    type: {
      type: String,
      default: 'button',
      validator: (prop: string) => typeof prop === typeof 'submit' || typeof prop === typeof 'button',
    },
  },
});
</script>

<style scoped lang="scss">
.button {
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  background: none;
  border: none;
  outline: none;
}
</style>
