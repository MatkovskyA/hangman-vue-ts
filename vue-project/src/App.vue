<template>
  <div id="app">
    <GameHeader />
    <div class="game-container">
      <GameFigureIcon />
      <GameWrongLetters />
      <GameTrueLetters />
    </div>

    <!-- Container for final message -->


    <!-- Notification -->
    <div class="notification-container show">
      <p>Вы уже вводили этот символ</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import GameHeader from './components/GameHeader.vue'
import GameFigureIcon from './components/GameFigureIcon.vue'
import GameWrongLetters from './components/GameWrongLetters.vue'
import GameTrueLetters from './components/GameTrueLetters.vue';
</script>
