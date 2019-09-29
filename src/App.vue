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
      <custom-button @buttonClick="savePdf('landscape')" text="PDF горизонталный" />
      <custom-button @buttonClick="savePdf('portrait')" text="PDF вертикальный" />
      <context-menu @cellChange="changeCell" @windowClose="closeContextMenu"
                    :localStyle="localStyle" :offset="contextMenuOffset"
                    v-if="contextMenuVisible" />
  </div>
</template>

<script>
import Vue from 'vue';
import TableCell from './components/table-cell.vue';
import ColorPicker from './components/pickers/color-picker.vue';
import BorderStylePicker from './components/pickers/border-style-picker.vue';
import BorderWidthPicker from './components/pickers/border-width-picker.vue';
import FontSizePicker from './components/pickers/font-size-picker.vue';
import RowsPicker from './components/pickers/rows-picker.vue';
import FontPicker from './components/pickers/font-picker.vue';
import CustomButton from './components/custom-button.vue';
import ContextMenu from './components/context-menu.vue';
import domain from '../settings';

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
      const table = this.$refs.table.innerHTML;
      const WinPrint = window.open('', '', `left=0,top=0,width=${window.screen.width},height=${window.screen.height},
      toolbar=0,scrollbars=0,status=0`);
      WinPrint.document.write(this.htmlString(table));
      WinPrint.document.close();
      WinPrint.focus();
      WinPrint.print();
      WinPrint.close();
    },
    savePdf(format) {
      const table = this.$refs.table.innerHTML;
      fetch(`https://${domain}/html-pdf`, {
        method: 'POST',
        mode: 'cors',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          html: this.htmlString(table),
          orientation: format,
        }),
      })
        .then((response) => {
          const reader = response.body.getReader();
          return new ReadableStream({
            start(controller) {
              return pump();
              function pump() {
                return reader.read().then(({ done, value }) => {
                  // When no more data needs to be consumed, close the stream
                  if (done) {
                    controller.close();
                    return;
                  }
                  // Enqueue the next data chunk into our target stream
                  controller.enqueue(value);
                  // eslint-disable-next-line consistent-return
                  return pump();
                });
              }
            },
          });
        })
        .then(stream => new Response(stream))
        .then(response => response.blob())
        .then(blob => URL.createObjectURL(blob))
        .then((url) => {
          const link = document.createElement('a');
          link.href = url;
          link.download = 'table.pdf';
          link.click();
        })
        .catch(err => console.error(err));
    },
    showContextMenu(value) {
      this.cellClicked = value;
      this.contextMenuVisible = true;
    },
    closeContextMenu() {
      this.cellClicked = '';
      this.contextMenuVisible = false;
    },
    htmlString(prtHtml) {
      let stylesHtml = '';
      const nodes = [...document.querySelectorAll('link[rel="stylesheet"], style')];
      nodes.forEach((node) => {
        if (node.href) return;
        stylesHtml += node.outerHTML;
      });
      const doc = `<!DOCTYPE html>
        <html>
          <head>
            ${stylesHtml}
          </head>
          <body>
            <div style="text-align: center; margin: 0px">
                ${prtHtml}
            </div>
          </body>
        </html>`;
      return doc;
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
