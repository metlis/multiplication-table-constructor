<template>
    <button @click="$emit('buttonClick')">{{text}}</button>
</template>

<script>
export default {
  name: 'custom-button',
  props: ['text'],
};
</script>

<style lang="stylus" scoped>
  button
    cursor pointer
    background-color #2ed67d
    border-radius 10%
    border none
    padding 0.5rem 1rem
</style>