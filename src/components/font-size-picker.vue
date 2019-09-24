<template>
    <div>
        <input type="range" class="custom-range" id="customRange1" v-model="value" min="0" max="60">
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

<style scoped>

</style>