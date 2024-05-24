<template>
  <MainScreen v-if="statusMatch === 'default'" @on-start="onHandleBeforeStart"/>
  <interact-screen v-if="statusMatch === 'match'" :cardsContext="settings.cardsContext" @onFinish="onGetResult"/>
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @on-start-again="statusMatch = 'default'"/>
  <copyright-screen/>
</template>

<script setup lang="ts">
import { shuffled } from '@/utils/array'

interface Settings {
  totalOfBlocks: number
  cardsContext: number[]
  startedAt: number | null
}
let settings = ref<Settings>({
  totalOfBlocks: 0,
  cardsContext: [],
  startedAt: null
})
let statusMatch = ref<any>('default')
let timer = ref<number>(0)

const onHandleBeforeStart = (config: {totalOfBlock: number}) => {
  settings.value.totalOfBlocks = config.totalOfBlock
  const firstCard = Array.from({ length: settings.value.totalOfBlocks / 2}, (_, i) => i + 1)
  const secondCard = [...firstCard]
  const cards = [...firstCard, ...secondCard]
  settings.value.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))))
  settings.value.startedAt = new Date().getTime()
  statusMatch.value = "match"
}

const onGetResult = () => {
  timer.value = new Date().getTime() - settings.value.startedAt!
  statusMatch.value = "result"
}
</script>
