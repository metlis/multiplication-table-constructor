<template>
    <div>
        <label for="fontSelect">Шрифт</label><br>
        <select id="fontSelect" v-model="font">
            <option v-for="font in fonts" :key="font">
                {{font}}
            </option>
        </select>
    </div>
</template>

<script>
export default {
  name: 'font-picker',
  data() {
    return {
      fonts: ['serif', 'sans-serif', 'monospace'],
      font: 'serif',
    };
  },
  watch: {
    font(val) {
      this.$emit('fontChange', val);
    },
  },
};
</script>

<style lang="stylus" scoped>
    label
      display inline-block
      margin-bottom 0.5rem
</style>