<template>
  <div>
      <div ref="table">
        <table-cell
          v-for="(item, index) in Array(10)"
          :id="index+1"
          :ref="index+1"
          :key="index+1"
          :number="index+1"
          :style="styleString"
          @cellClick="showContextMenu"
        />
      </div>
      <color-picker @colorChange="changeColor" />
      <border-style-picker @borderChange="changeBorderStyle" />
      <border-width-picker @borderChange="changeBorderWidth" />
      <font-size-picker @fontChange="changeFontSize" />
      <rows-picker @rowsChange="changeWidth" />
      <font-picker @fontChange="changeFont" />
      <custom-button @buttonClick="printTable" text="Печать таблицы" />
      <context-menu @cellChange="changeCell" @windowClose="closeContextMenu"
                    :localStyle="localStyle" :offset="contextMenuOffset"
                    v-if="contextMenuVisible" />
  </div>
</template>

<script>
import Vue from 'vue';
import TableCell from './components/table-cell.vue';
import ColorPicker from './components/color-picker.vue';
import BorderStylePicker from './components/border-style-picker.vue';
import BorderWidthPicker from './components/border-width-picker.vue';
import FontSizePicker from './components/font-size-picker.vue';
import RowsPicker from './components/rows-picker.vue';
import FontPicker from './components/font-picker.vue';
import CustomButton from './components/custom-button.vue';
import ContextMenu from './components/context-menu.vue';

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
    ContextMenu,
  },
  data() {
    return {
      color: '',
      borderStyle: 'solid',
      borderWidth: 1,
      fontSize: 14,
      fontFamily: '',
      width: 17,
      contextMenuVisible: false,
      cellClicked: '',
      localStyles: {},
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
      this.fontFamily = value;
    },
    changeCell(cellAttributes) {
      // revert local settings
      const cell = this.$refs[this.cellClicked][0].$el;
      if (Object.values(cellAttributes).every(val => val === '')) {
        if (this.localStyles[this.cellClicked]) Vue.delete(this.localStyles, this.cellClicked);
        this.contextMenuVisible = false;
        cell.style = this.styleString;
      // apply local settings
      } else {
        this.localStyles[this.cellClicked] = cellAttributes;
        Object.keys(cellAttributes).forEach((attr) => {
          if (cellAttributes[attr]) {
            cell.style[attr] = ['borderWidth', 'fontSize'].includes(attr) ? `${cellAttributes[attr]}px` : cellAttributes[attr];
          }
        });
      }
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
    showContextMenu(value) {
      this.cellClicked = value;
      this.contextMenuVisible = true;
    },
    closeContextMenu() {
      this.cellClicked = '';
      this.contextMenuVisible = false;
    },
  },
  computed: {
    styleString() {
      return `color: ${this.color}; border-style: ${this.borderStyle}; border-color: ${this.color};
      border-width: ${this.borderWidth}px; font-size: ${this.fontSize}px; font-family: ${this.fontFamily};
      width: ${this.width}%`;
    },
    localStyle() {
      if (this.localStyles[this.cellClicked]) {
        return this.localStyles[this.cellClicked];
      }
      return '';
    },
    contextMenuOffset() {
      return this.$refs.table.offsetTop;
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
