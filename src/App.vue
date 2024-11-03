<template>
  <div id="app">
    <SchafkopfGame />
  </div>
</template>

<script>
import SchafkopfGame from './components/SchafkopfGame.vue'; // Обновите импорт

export default {
  components: {
    SchafkopfGame,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

