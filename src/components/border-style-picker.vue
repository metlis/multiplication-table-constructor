<template>
    <span>
        <label for="styles">Стиль границ</label><br>
        <span id="styles" >
            <div
                v-for="style in borderStyles"
                :style="`border-style: ${style}`"
                @click="handleClick(style)"
            />
        </span>
    </span>
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
      cursor pointer
      display inline-block
</style>