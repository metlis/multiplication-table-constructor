<template>
    <div @click="handleCellClick(number)">
        <p
            v-for="(item, index) in Array(10)"
            :key="index"
        >
            {{number*(index+1)}}:{{number}}={{number*(index+1)/number}}
        </p>
    </div>
</template>

<script>
export default {
  name: 'TableCell',
  props: ['id', 'number'],
  methods: {
    handleCellClick(num) {
      this.$emit('cellClick', this.id);
    },
  },
};
</script>

<style lang="stylus" scoped>
  div
    padding 0.5rem
    text-align center
    overflow hidden
    display inline-block
    margin-right 1%
    box-sizing border-box
    cursor pointer
    p
      padding 0px
      margin 0px
      :last-child
        margin-bottom 0px
</style>