<template>
    <div>
        <label for="styles">Estilos de borde</label><br>
        <div id="styles" >
            <div
                v-for="style in borderStyles"
                :class="$style.container"
                :style="generateStyle(style)"
                @click="handleClick(style)"
            />
        </div>
    </div>
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

<style lang="stylus" module>
    .container
      width 3rem
      height 3rem
      margin 0.5rem
      cursor pointer
      display inline-block
      opacity 0.5
</style>