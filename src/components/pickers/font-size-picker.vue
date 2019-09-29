<template>
    <div>
        <label for="customRange2">Font size ({{textValue}})</label>
        <input type="range" class="custom-range-font" id="customRange2"
               v-model="fontSizeValue" min="1" max="60">
    </div>
</template>

<script>
export default {
  name: 'font-size-picker',
  props: ['localFontSize'],
  data() {
    return {
      fontSizeValue: 14,
    };
  },
  computed: {
    textValue() {
      return `${this.fontSizeValue}px`;
    },
  },
  watch: {
    fontSizeValue(val) {
      this.$emit('fontChange', val);
    },
  },
  created() {
    if (this.localFontSize) this.fontSizeValue = this.localFontSize;
  },
};
</script>

<style lang="stylus" scoped>
  input
    width 100%
    text-align center
</style>