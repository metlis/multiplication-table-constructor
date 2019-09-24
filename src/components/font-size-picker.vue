<template>
    <div>
        <input type="range" class="custom-range" id="customRange1" v-model="value" min="0" max="40">
    </div>
</template>

<script>
export default {
  name: 'font-size-picker',
  data() {
    return {
      value: 20,
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