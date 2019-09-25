<template>
    <span>
        <label for="styles">Стиль границ</label><br>
        <span id="styles" >
            <div
                v-for="style in borderStyles"
                :style="generateStyle(style)"
                @click="handleClick(style)"
            />
        </span>
    </span>
</template>

<script>
export default {
  name: 'border-style-picker',
  props: ['localBorderStyle'],
  data() {
    return {
      borderStyles: ['dotted', 'dashed', 'solid', 'double', 'groove', 'ridge', 'inset', 'outset'],
      chosenBorder: '',
    };
  },
  methods: {
    handleClick(value) {
      this.chosenBorder = value;
      this.$emit('borderChange', value);
    },
    generateStyle(value) {
      if (value === this.chosenBorder) return `border-style: ${value}; opacity: 1`;
      return `border-style: ${value}`;
    },
  },
  created() {
    if (this.localBorderStyle) this.chosenBorder = this.localBorderStyle;
  },
};
</script>

<style lang="stylus" scoped>
    div
      width 3rem
      height 3rem
      margin 0.5rem
      cursor pointer
      display inline-block
      opacity 0.5
</style>