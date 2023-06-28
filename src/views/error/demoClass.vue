<template>
    <button class="square">{{"data.id===-1"?'':data.value}}</button>
</template>

<script setup lang="ts">


import {onUpdated, watch} from "vue";

type SquareType = 'X' | 'O' | ''
interface obj{
    value:SquareType,
    id:number
}
interface Props {
    data:obj
}

const props=defineProps<Props>()
console.log(props.data)

watch(props.data,(val)=>{
    console.log(val)
})

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
