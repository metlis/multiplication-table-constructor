<template>
    <div>
        <label for="customRange2">Размер шрифта</label>
        <input type="range" class="custom-range-font" id="customRange2"
               v-model="value" min="0" max="60">
    </div>
</template>

<script>
export default {
  name: 'font-size-picker',
  data() {
    return {
      value: 30,
    };
  },
  watch: {
    value(val) {
      this.$emit('fontChange', val);
    },
  },
};
</script>

<style lang="stylus" scoped>
  input
    width 100%
    text-align center
</style>