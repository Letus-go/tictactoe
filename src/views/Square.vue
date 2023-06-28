<template>
    <button class="square">{{value}}</button>
</template>

<script setup lang="ts">
interface Props {
    value: string
}

defineProps<Props>()


</script>

<style scoped lang="less">
.square {
  width: 60px;
  height: 60px;
  font-size: 30px;
  background-color: #fff;
  border: 1px solid #333;
  margin-top: -1px;
  margin-right: -1px;
  float: left;
}
</style>
