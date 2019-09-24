<template>
  <div>
    <table-cell
      v-for="(item, index) in Array(10)"
      :key="index"
      :number="index+1"
      :style="styleString"
    />
    <color-picker @colorChange="changeColor" />
    <border-style-picker @borderChange="changeBorderStyle" />
    <border-width-picker @borderChange="changeBorderWidth" />
    <font-size-picker @fontChange="changeFontSize" />
  </div>
</template>

<script>
import TableCell from './components/table-cell.vue';
import ColorPicker from './components/color-picker.vue';
import BorderStylePicker from './components/border-style-picker.vue';
import BorderWidthPicker from './components/border-width-picker.vue';
import FontSizePicker from './components/font-size-picker.vue';

export default {
  name: 'App',
  components: {
    TableCell, ColorPicker, BorderStylePicker, BorderWidthPicker, FontSizePicker,
  },
  data() {
    return {
      color: '',
      borderStyle: 'solid',
      borderWidth: 1,
      fontSize: 14,
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
  },
  computed: {
    styleString() {
      return `color: ${this.color}; border-style: ${this.borderStyle}; border-color: ${this.color};
      border-width: ${this.borderWidth}px; font-size: ${this.fontSize}px`;
    },
  },
};
</script>

<style scoped>

</style>
