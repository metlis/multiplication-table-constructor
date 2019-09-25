<template>
    <div :class="$style.context">
        <color-picker :localColor="localColor" @colorChange="changeColor" /><br>
        <border-style-picker :localBorderStyle="localBorderStyle"
                             @borderChange="changeBorderStyle" />
        <border-width-picker :localBorderWidth="localBorderWidth"
                             @borderChange="changeBorderWidth" />
        <font-size-picker :localFontSize="localFontSize" @fontChange="changeFontSize" />
        <font-picker :localFontFamily="localFontFamily" @fontChange="changeFontFamily" /><br>
        <custom-button @buttonClick="revertChanges" color="#ebbdb5" text="Сбросить"
                       v-if="color || borderStyle || borderWidth || fontFamily || fontSize" />
        <custom-button @buttonClick="closeWindow" color="#ebbdb5" text="Закрыть" />
    </div>
</template>

<script>
import ColorPicker from './color-picker.vue';
import BorderStylePicker from './border-style-picker.vue';
import BorderWidthPicker from './border-width-picker.vue';
import FontPicker from './font-picker.vue';
import CustomButton from './custom-button.vue';
import FontSizePicker from './font-size-picker.vue';

export default {
  name: 'context-menu',
  components: {
    ColorPicker, BorderStylePicker, FontPicker, CustomButton, BorderWidthPicker, FontSizePicker,
  },
  props: ['localStyle'],
  data() {
    return {
      color: '',
      borderStyle: '',
      borderWidth: '',
      fontFamily: '',
      fontSize: '',
    };
  },
  methods: {
    changeColor(value) {
      this.color = value;
    },
    changeBorderStyle(value) {
      this.borderStyle = value;
    },
    changeFontFamily(value) {
      this.fontFamily = value;
    },
    changeBorderWidth(value) {
      this.borderWidth = value;
    },
    changeFontSize(value) {
      this.fontSize = value;
    },
    closeWindow() {
      this.$emit('windowClose');
    },
    revertChanges() {
      this.color = '';
      this.borderStyle = '';
      this.borderWidth = '';
      this.fontFamily = '';
      this.fontSize = '';
    },
    submitChanges() {
      this.$emit('cellChange', {
        color: this.color,
        borderStyle: this.borderStyle,
        borderWidth: this.borderWidth,
        fontFamily: this.fontFamily,
        fontSize: this.fontSize,
      });
    },
  },
  computed: {
    localColor() {
      if (this.localStyle && this.localStyle.color) {
        return this.localStyle.color;
      }
      return '';
    },
    localBorderStyle() {
      if (this.localStyle && this.localStyle.borderStyle) {
        return this.localStyle.borderStyle;
      }
      return '';
    },
    localBorderWidth() {
      if (this.localStyle && this.localStyle.borderWidth) {
        return this.localStyle.borderWidth;
      }
      return '';
    },
    localFontSize() {
      if (this.localStyle && this.localStyle.fontSize) {
        return this.localStyle.fontSize;
      }
      return '';
    },
    localFontFamily() {
      if (this.localStyle && this.localStyle.fontFamily) {
        return this.localStyle.fontFamily;
      }
      return '';
    },
  },
  watch: {
    color() {
      this.submitChanges();
    },
    borderStyle() {
      this.submitChanges();
    },
    borderWidth() {
      this.submitChanges();
    },
    fontFamily() {
      this.submitChanges();
    },
    fontSize() {
      this.submitChanges();
    },
  },
};
</script>

<style lang="stylus" module>
  .context
      width 20rem !important
      padding 1rem
      border 1px solid lightgray
      position absolute
      top 0
      left 0
      right 0
      margin-left auto
      margin-right auto
      background-color white
      z-index 9999
</style>