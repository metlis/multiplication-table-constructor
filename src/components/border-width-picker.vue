<template>
    <div>
        <label for="customRange1">Ширина границ ({{textValue}})</label>
        <input type="range" class="custom-range-width" id="customRange1"
               v-model="widthValue" min="1" max="20">
    </div>
</template>

<script>
export default {
  name: 'border-width-picker',
  props: ['localBorderWidth'],
  data() {
    return {
      widthValue: 1,
    };
  },
  computed: {
    textValue() {
      return `${this.widthValue}px`;
    },
  },
  watch: {
    widthValue(val) {
      this.$emit('borderChange', val);
    },
  },
  created() {
    if (this.localBorderWidth) this.widthValue = this.localBorderWidth;
  },
};
</script>

<style lang="stylus" scoped>
  input
    width 100%
    text-align center
</style>