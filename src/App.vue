<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12">
        <div style="display: flex; flex-wrap: wrap; justify-content: center">
          <table-cell
            v-for="(item, index) in Array(10)"
            :key="index"
            :number="index+1"
            :style="styleString"
          />
        </div>
        <color-picker @colorChange="changeColor" />
        <border-style-picker @borderChange="changeBorderStyle" />
        <border-width-picker @borderChange="changeBorderWidth" />
        <font-size-picker @fontChange="changeFontSize" />
        <rows-picker @rowsChange="changeWidth" />
      </div>
    </div>
  </div>
</template>

<script>
import TableCell from './components/table-cell.vue';
import ColorPicker from './components/color-picker.vue';
import BorderStylePicker from './components/border-style-picker.vue';
import BorderWidthPicker from './components/border-width-picker.vue';
import FontSizePicker from './components/font-size-picker.vue';
import RowsPicker from './components/rows-picker.vue';

export default {
  name: 'App',
  components: {
    TableCell, ColorPicker, BorderStylePicker, BorderWidthPicker, FontSizePicker, RowsPicker,
  },
  data() {
    return {
      color: '',
      borderStyle: 'solid',
      borderWidth: 1,
      fontSize: 14,
      width: 18,
    };
  },
  methods: {
    changeColor(value) {
      this.color = value;
    },
    changeBorderStyle(value) {
      this.borderStyle = value;
    },
    changeBorderWidth(value) {
      this.borderWidth = value;
    },
    changeFontSize(value) {
      this.fontSize = value;
    },
    changeWidth(value) {
      this.width = 100 / value - 2;
    },
  },
  computed: {
    styleString() {
      return `color: ${this.color}; border-style: ${this.borderStyle}; border-color: ${this.color};
      border-width: ${this.borderWidth}px; font-size: ${this.fontSize}px; width: ${this.width}%`;
    },
  },
};
</script>

<style lang="stylus" scoped>
  div
    margin-top 0.3rem
    margin-bottom 1rem
    text-align center
</style>
