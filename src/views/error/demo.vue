<template>
    <div class="ticktacktoe">
        <!--    <div class="row">-->
        <!--      <Square @click="handleClick(0)" :value="list[0]" />-->
        <!--      <Square @click="handleClick(1)" :value="list[1]" />-->
        <!--      <Square @click="handleClick(2)" :value="list[2]" />-->
        <!--    </div>-->
        <!--    <div class="row">-->
        <!--      <Square @click="handleClick(3)" :value="list[3]" />-->
        <!--      <Square @click="handleClick(4)" :value="list[4]" />-->
        <!--      <Square @click="handleClick(5)" :value="list[5]" />-->
        <!--    </div>-->
        <!--    <div class="row">-->
        <!--      <Square @click="handleClick(6)" :value="list[6]" />-->
        <!--      <Square @click="handleClick(7)" :value="list[7]" />-->
        <!--      <Square @click="handleClick(8)" :value="list[8]" />-->
        <!--    </div>-->
        <div class="ticktacktoeBox">
            <div v-for="(item,index) in list" :key="index">
                <Square @click="handleClick(index)" :data="item" />
            </div>
        </div>
        <div class="ticktacktoeDone">
            <el-button type="info" round @click="repentantChess" :disabled="disabled">悔棋</el-button>
            <el-button type="warning" round @click="confirmChess">重置</el-button>
            <el-button type="warning" round @click="sure">确定</el-button>
        </div>
    </div>

</template>

<script setup lang="ts">
import Square from '@/views/error/demoClass.vue'
import {reactive, ref} from 'vue'
import { ElMessage, ElMessageBox } from 'element-plus'

type SquareType = 'X' | 'O' | ''
// const list = reactive<SquareType[]>(Array(9).fill(''))
const list = reactive<{ value:SquareType,id:number }[]>(Array(9).fill({value:'',id:-1}))
const xIsNext = ref<boolean>(true)
//方格是否选中

const disabled=ref<boolean>(true)
//悔棋的索引
const repentantIndex=ref<number>(-1)


const handleClick = (index: number) => {
    repentantIndex.value=index
    list.forEach((item,index)=>{
        list[index].value=''
    })
    list[index].value = xIsNext.value ? 'X' : 'O'
}
const calculatingVictory = (square:{value:SquareType,id:number }[]): string | null => {
    const list = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
    ]

    for (let i = 0; i < list.length; i++) {
        const [a, b, c] = list[i]
        if (square[a] && square[a] === square[b] && square[a] === square[c]) {
            ElMessage({
                message: '恭喜你，你赢了 !',
                type: 'success',
            })
            disabled.value=true
            return square[a].value
        }
    }
    return null
}
// 重置
const confirmChess=()=>{
    list.forEach((item,index)=> list[index].value='')
}
// 确定
const sure=()=>{
    if ( calculatingVictory(list)) return
    disabled.value=false
    list[repentantIndex.value].value = xIsNext.value ? 'X' : 'O'
    list[repentantIndex.value].id=repentantIndex.value
    xIsNext.value = !xIsNext.value
    calculatingVictory(list)
}
//悔棋
const repentantChess=():void=>{
    ElMessageBox.confirm(
        '是否确认悔棋？',
        '提示',
        {
            confirmButtonText: '确认',
            cancelButtonText: '取消',
            type: 'warning',
        }
    )
        .then(() => {
            if(list.find((item,index)=>index===repentantIndex.value)){
                list[repentantIndex.value].value=''
            }
            ElMessage({
                type: 'success',
                message: '请重新选择',
            })
        })
        .catch(() => {
            ElMessage({
                type: 'info',
                message: '取消',
            })
        })
        .finally(()=>{
            disabled.value=true
        })
}


</script>

<style scoped lang="less">
.ticktacktoe{
  display: flex;
  flex-direction: column;
}
.ticktacktoeBox {
  width: 180px;
}
.ticktacktoeDone{
  margin: 10px auto;
}
</style>
