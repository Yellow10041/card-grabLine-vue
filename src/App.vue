<script setup lang="ts">
// import HelloWorld from './components/HelloWorld.vue'
import CardGrab from './components/CardGrab.vue';
import Rank from "./components/Rank/Rank.vue"
</script>

<template>
  <div class="App">
    <div class="App_cardsGrab">
      <CardGrab theme="dark" />
      <CardGrab theme="light" />
    </div>
    <div class="App_rank">
      <Rank theme="light" />
      <Rank theme="dark" />
    </div>
  </div>
</template>

<style scoped lang="scss">
.App {
  display: flex;
  flex-direction: column;
  gap: 20px;
  width: 100%;
  height: 100%;
  padding: 50px;
}

.App_cardsGrab {
  display: flex;
  justify-content: center;
  gap: 30px;
}

.App_rank {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  width: 100%;
}
</style>
