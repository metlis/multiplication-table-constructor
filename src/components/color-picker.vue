<template>
    <div>
        <label for="colorPicker">Color</label><br>
        <slider-picker id="colorPicker" v-model="colors" />
    </div>
</template>

<script>
import { Slider } from 'vue-color';

export default {
  name: 'color-picker',
  components: {
    'slider-picker': Slider,
  },
  props: ['localColor'],
  data() {
    return {
      colors: {
        hex: '',
      },
    };
  },
  watch: {
    'colors.hex': function (val) {
      this.$emit('colorChange', val);
    },
  },
  created() {
    if (this.localColor) this.colors.hex = this.localColor;
  },
};
</script>

<style lang="stylus" scoped>
  div
    width 100%
    text-align center
  label
    display inline-block
    margin-bottom 0.5rem
</style>