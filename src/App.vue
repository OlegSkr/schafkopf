<template>
  <v-app>
    <v-container fluid class="app-container">
      <v-toolbar color="primary" dark class="toolbar">
        <v-toolbar-title>Schafkopf Spiel</v-toolbar-title>
      </v-toolbar>
      <v-row justify="center" class="game-row">
        <schafkopf-game />
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import SchafkopfGame from './components/SchafkopfGame.vue';

export default {
  name: 'App',
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
}

.app-container {
  padding: 0; /* Убираем внутренние отступы */
  margin: 0; /* Убираем внешние отступы */
}

.toolbar {
  width: 100%; /* Убедитесь, что toolbar занимает всю ширину */
}

.game-row {
  width: 100%; /* Устанавливаем ширину для v-row, чтобы она занимала всю ширину */
}
</style>
