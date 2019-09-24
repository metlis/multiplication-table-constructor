<template>
    <div>
        <input type="range" class="custom-range" id="customRange1" v-model="value" min="0" max="20">
    </div>
</template>

<script>
export default {
  name: 'border-width-picker',
  data() {
    return {
      value: 10,
    };
  },
  watch: {
    value(val) {
      this.$emit('borderChange', val);
    },
  },
};
</script>

<style scoped>

</style>