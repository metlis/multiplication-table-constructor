<template>
    <button @click="$emit('buttonClick')" :style="bgColor">
        {{text}}
    </button>
</template>

<script>
export default {
  name: 'custom-button',
  props: ['text', 'color'],
  computed: {
    bgColor() {
      if (this.color) return `background-color: ${this.color}`;
      return 'background-color: #c5f0c5';
    },
  },
};
</script>

<style lang="stylus" scoped>
  button
    cursor pointer
    border-radius 10%
    border none
    padding 0.5rem 1rem
</style>