<template>
    <div>
        <label for="options">Número de celdas en la fila</label><br>
        <div v-for="option in rowsOptions" :key="option" id="options">
            <input type="radio" name="inlineRadioOptions"
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
      rowsOptions: [1, 2, 5],
      picked: 5,
    };
  },
  watch: {
    picked(val) {
      this.$emit('rowsChange', val);
    },
  },
};
</script>

<style lang="stylus" scoped>
  div
    display inline-block
    div
      margin-top 0.5rem
</style>