<template>
  <div>
      <div ref="table">
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
      <font-picker @fontChange="changeFont" />
      <custom-button @buttonClick="printTable" text="Печать" />
  </div>
</template>

<script>
import TableCell from './components/table-cell.vue';
import ColorPicker from './components/color-picker.vue';
import BorderStylePicker from './components/border-style-picker.vue';
import BorderWidthPicker from './components/border-width-picker.vue';
import FontSizePicker from './components/font-size-picker.vue';
import RowsPicker from './components/rows-picker.vue';
import FontPicker from './components/font-picker.vue';
import CustomButton from './components/custom-button.vue';

export default {
  name: 'App',
  components: {
    TableCell,
    ColorPicker,
    BorderStylePicker,
    BorderWidthPicker,
    FontSizePicker,
    RowsPicker,
    CustomButton,
    FontPicker,
  },
  data() {
    return {
      color: '',
      borderStyle: 'solid',
      borderWidth: 1,
      fontSize: 14,
      fontFamily: '',
      width: 17,
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
      this.width = 100 / value - 3;
    },
    changeFont(value) {
      console.log(value);
      this.fontFamily = value;
    },
    printTable() {
      const prtHtml = this.$refs.table.innerHTML;
      let stylesHtml = '';
      const nodes = [...document.querySelectorAll('link[rel="stylesheet"], style')];
      nodes.forEach((node) => {
        stylesHtml += node.outerHTML;
      });
      const WinPrint = window.open('', '', `left=0,top=0,width=${window.screen.width},height=${window.screen.height},
      toolbar=0,scrollbars=0,status=0`);
      WinPrint.document.write(`<!DOCTYPE html>
        <html>
          <head>
            ${stylesHtml}
          </head>
          <body>
            <div style="text-align: center; margin: 0px">
                ${prtHtml}
            </div>
          </body>
        </html>`);
      WinPrint.document.close();
      WinPrint.focus();
      WinPrint.print();
      WinPrint.close();
    },
  },
  computed: {
    styleString() {
      return `color: ${this.color}; border-style: ${this.borderStyle}; border-color: ${this.color};
      border-width: ${this.borderWidth}px; font-size: ${this.fontSize}px; font-family: ${this.fontFamily};
      width: ${this.width}%`;
    },
  },
};
</script>

<style lang="stylus" scoped>
  div
    width 100%
    margin-top 0.5rem
    margin-bottom 0.5rem
    text-align center
</style>
