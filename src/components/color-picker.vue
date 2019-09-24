<template>
    <div>
        <label for="colorPicker">Цвет</label><br>
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
  data() {
    return {
      colors: {
        hex: '#000000',
      },
    };
  },
  watch: {
    'colors.hex': function (val) {
      this.$emit('colorChange', val);
    },
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