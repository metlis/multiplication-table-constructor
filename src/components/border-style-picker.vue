<template>
    <div>
        <div
            v-for="style in borderStyles"
            :style="`border-style: ${style}`"
            @click="handleClick(style)"
        />
    </div>
</template>

<script>
export default {
  name: 'border-style-picker',
  data() {
    return {
      borderStyles: ['dotted', 'dashed', 'solid', 'double', 'groove', 'ridge', 'inset', 'outset'],
    };
  },
  methods: {
    handleClick(value) {
      this.$emit('borderChange', value);
    },
  },
};
</script>

<style lang="stylus" scoped>
    div
      width 50px
      height 50px
      margin 10px
</style>