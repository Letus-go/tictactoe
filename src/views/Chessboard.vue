<template>
  <div>
    <div class="row">
      <Square @click="handleClick(0)" :value="list[0]" />
      <Square @click="handleClick(1)" :value="list[1]" />
      <Square @click="handleClick(2)" :value="list[2]" />
    </div>
    <div class="row">
      <Square @click="handleClick(3)" :value="list[3]" />
      <Square @click="handleClick(4)" :value="list[4]" />
      <Square @click="handleClick(5)" :value="list[5]" />
    </div>
    <div class="row">
      <Square @click="handleClick(6)" :value="list[6]" />
      <Square @click="handleClick(7)" :value="list[7]" />
      <Square @click="handleClick(8)" :value="list[8]" />
    </div>
  </div>
</template>

<script setup lang="ts">
import Square from '@/views/Square.vue'
import { reactive, ref } from 'vue'
type SquareType = 'X' | 'O' | ''
const list = reactive<SquareType[]>(Array(9).fill(''))
const xIsNext = ref<boolean>(true)

const handleClick = (index: number) => {
  if (list[index] || calculatingVictory(list)) return
  list[index] = xIsNext.value ? 'X' : 'O'
  xIsNext.value = !xIsNext.value
  console.log(calculatingVictory(list))
}
const calculatingVictory = (square: SquareType[]): string | null => {
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
      return square[a]
    }
  }
  return null
}
</script>

<style scoped lang="less"></style>
