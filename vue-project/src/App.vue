<script setup lang="ts">
import { ref, computed } from 'vue'
import GameHeader from './components/GameHeader.vue'
import GameFigureIcon from './components/GameFigureIcon.vue'
import GameWrongLetters from './components/GameWrongLetters.vue'
import GameTrueLetters from './components/GameTrueWord.vue'
import GamePopup from './components/GamePopup.vue'
import GameNotification from './components/GameNotification.vue'

const word = ref('василий')
const letters = ref<String[]>([])
const correctLetters = computed(() => letters.value.filter(x => word.value.includes(x)))
const wrongtLetters = computed(() => letters.value.filter(x => !word.value.includes(x)))
const notification = ref<InstanceType<typeof GameNotification> | null>(null)

window.addEventListener('keydown', ({ key }) => {
  if(letters.value.includes(key)) {
    notification.value?.open()
    setTimeout(() => notification.value?.close(), 2000)
    return
  };

  if(/[а-яА-ЯёЁ]/.test(key)) {
    letters.value.push(key.toLowerCase())
  }
})
</script>

<template>
  <div id="app">
    {{ word }}
    {{ letters }}
    {{ correctLetters }}
    {{ wrongtLetters }}
    <GameHeader />
    <div class="game-container">
      <GameFigureIcon />
      <GameWrongLetters :wrong-letters="wrongtLetters"/>
      <GameTrueLetters :word="word" :correct-letters="correctLetters"/>
    </div>

    <GamePopup v-if="false" />
    <GameNotification ref="notification"/>
  </div>
</template>
