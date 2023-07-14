<template>
  <div>
    <textarea cols="30" rows="10" v-model="inputValue"></textarea>
    <br>
    <button @click="addPlayer">Add</button>
    <button @click="clearPlayer">clear</button>
    <br>
    <button @click="shuffle">Shuffle Start</button>
    <button @click="stop">Shuffle Stop</button>
  </div>
  <ul class="player-list">
    <li v-for="player in players" :key="player">
      {{ player }}
    </li>
  </ul>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const inputValue = ref('')
const players = ref<string[]>([])
const timerID = ref<null|number>(null)

const addPlayer = () => {
  const arr = inputValue.value.split(/\n/)
  players.value = [...arr]
  inputValue.value = ''
}

const clearPlayer = () => {
  players.value = []
}

const shuffle = () => {
  if (players.value.length < 2) {
    alert('2つ以上入力してください')
    return
  }
  if (timerID.value !== null) return
  timerID.value = setInterval(() => {
    for (let i = players.value.length - 1; i > 0; i--) {
      let j = Math.floor(Math.random() * (i + 1));
      [players.value[i], players.value[j]] = [players.value[j], players.value[i]];
    }
  }, 100)
}

const stop = () => {
  if (timerID.value !== null) {
    clearInterval(timerID.value)
    timerID.value = null
  }
}
</script>

<style>
.player-list {
  list-style: none;
}
</style>