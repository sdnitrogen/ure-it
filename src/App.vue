<script setup lang="ts">
import { ref, computed } from "vue"

const gamestate = ref(0)
const lucky = ref(false)
const notit = computed(() => (lucky.value ? "😭" : "😁"))
const it = computed(() => (lucky.value ? "😁" : "😭"))
const count = ref(16)
const endMsg = computed(() =>
  lucky.value ? "Congratulations! You're it! 🥳" : "You're it! 🫵😆"
)

const setLucky = () => (lucky.value = true)
const setUnlucky = () => (lucky.value = false)

const board = ref([
  ["", "", "", ""],
  ["", "", "", ""],
  ["", "", "", ""],
  ["", "", "", ""],
])

const makeMove = (x: number, y: number) => {
  if (board.value[x][y] !== "") return
  gamestate.value = 1
  const roll = Math.floor(Math.random() * count.value)

  if (roll === 0) {
    board.value[x][y] = it.value
    count.value = 16
    gamestate.value = 2
    return
  }
  board.value[x][y] = notit.value
  count.value -= 1
}

const resetGame = () => {
  board.value = [
    ["", "", "", ""],
    ["", "", "", ""],
    ["", "", "", ""],
    ["", "", "", ""],
  ]
  gamestate.value = 0
}
</script>

<template>
  <main class="pt-8 text-center">
    <h1 class="mb-4 text-3xl font-bold uppercase">U're it</h1>
    <div class="inline-flex text-xl mb-8">
      <p>I'm feeling&nbsp;</p>
      <span
        class="cursor-pointer"
        :class="{
          'underline italic font-bold': lucky,
          'pointer-events-none': gamestate !== 0,
        }"
        @click="setLucky"
        >lucky</span
      >
      <p>&nbsp;/&nbsp;</p>
      <span
        class="cursor-pointer"
        :class="{
          'underline italic font-bold': !lucky,
          'pointer-events-none': gamestate !== 0,
        }"
        @click="setUnlucky"
        >unlucky</span
      >
    </div>
    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="makeMove(x, y)"
          class="border border-white flex items-center justify-center w-20 h-20 hover:bg-gray-500 text-4xl cursor-pointer"
          :class="gamestate === 2 && 'pointer-events-none'">
          {{ cell }}
        </div>
      </div>
    </div>
    <h2 v-if="gamestate === 2" class="text-4xl font-bold mb-8">{{ endMsg }}</h2>
    <button
      @click="resetGame"
      class="px-4 py-2 border-none bg-red-500 hover:bg-red-600 rounded uppercase font-bold duration-300">
      Reset Game
    </button>
  </main>
</template>

<style scoped></style>
