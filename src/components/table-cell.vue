<template>
    <div @click="handleCellClick(number)">
        <p
            v-for="(item, index) in Array(10)"
            :key="index"
            @click="handleRowClick(index+1)"
        >
            {{number}}*{{index+1}}={{number*(index+1)}}
        </p>
    </div>
</template>

<script>
export default {
  name: 'TableCell',
  props: ['number'],
  methods: {
    handleCellClick(num) {
      console.log(num);
    },
    handleRowClick(num) {
      console.log(num);
    },
  },
};
</script>

<style scoped>

</style>