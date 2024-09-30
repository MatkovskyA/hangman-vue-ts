<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import GameHeader from './components/GameHeader.vue'
import GameFigureIcon from './components/GameFigureIcon.vue'
import GameWrongLetters from './components/GameWrongLetters.vue'
import GameTrueLetters from './components/GameTrueWord.vue'
import GamePopup from './components/GamePopup.vue'
import GameNotification from './components/GameNotification.vue'

const word = ref('василий')
const letters = ref<String[]>([])
const correctLetters = computed(() => letters.value.filter((x) => word.value.includes(x)))
const wrongtLetters = computed(() => letters.value.filter((x) => !word.value.includes(x)))
const isLose = computed(() => wrongtLetters.value.length === 6)
const isWin = computed(() => [...word.value].every(x => correctLetters.value.includes(x)))
const notification = ref<InstanceType<typeof GameNotification> | null>(null)
const popup = ref<InstanceType<typeof GamePopup> | null>(null)

watch(wrongtLetters, () => {
  if(isLose.value) {
    popup.value?.open('lose')
  }
})

watch(correctLetters, () => {
  if(isWin.value) {
    popup.value?.open('win')
  }
})

window.addEventListener('keydown', ({ key }) => {
  if(isLose.value || isWin.value) {
    return
  }
  if(letters.value.includes(key)) {
    notification.value?.open()
    setTimeout(() => notification.value?.close(), 2000)
    return
  };

  if(/[а-яА-ЯёЁ]/.test(key)) {
    letters.value.push(key.toLowerCase())
  }
})

const restart = () => {
  letters.value = [];
  popup.value?.close()
}
</script>

<template>
  <div id="app">
    <GameHeader />
    <div class="game-container">
      <GameFigureIcon :wrong-letters-count="wrongtLetters.length"/>
      <GameWrongLetters :wrong-letters="wrongtLetters"/>
      <GameTrueLetters :word="word" :correct-letters="correctLetters"/>
    </div>

    <GamePopup ref="popup" :word="word" @restart="restart"/>
    <GameNotification ref="notification"/>
  </div>
</template>
