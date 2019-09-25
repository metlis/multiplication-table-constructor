<template>
    <div>
        <label for="fontSelect">Font type</label><br>
        <select id="fontSelect" v-model="activeFont">
            <option v-for="font in fonts" :key="font">
                {{font}}
            </option>
        </select>
    </div>
</template>

<script>
export default {
  name: 'font-picker',
  props: ['localFontFamily'],
  data() {
    return {
      fonts: ['serif', 'sans-serif', 'monospace'],
      activeFont: 'serif',
    };
  },
  watch: {
    activeFont(val) {
      this.$emit('fontChange', val);
    },
  },
  created() {
    if (this.localFontFamily) this.activeFont = this.localFontFamily;
  },
};
</script>

<style lang="stylus" scoped>
    label
      display inline-block
      margin-bottom 0.5rem
</style>