<template>
    <div>
        <slider-picker v-model="colors" />
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

<style scoped>

</style>