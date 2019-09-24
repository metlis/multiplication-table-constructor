<template>
    <div>
        <div class="form-check form-check-inline" v-for="option in rowsOptions" :key="option">
            <input class="form-check-input" type="radio" name="inlineRadioOptions"
                   :id="option" :value="option" v-model="picked">
            <label class="form-check-label" :for="option">{{option}}</label>
        </div>
    </div>
</template>

<script>
export default {
  name: 'rows-picker',
  data() {
    return {
      rowsOptions: [1, 2, 5, 10],
      picked: '',
    };
  },
  watch: {
    picked(val) {
      this.$emit('rowsChange', val);
    },
  },
};
</script>

<style scoped>

</style>