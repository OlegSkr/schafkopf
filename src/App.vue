<template>
  <v-app>
    <v-container>
      <v-toolbar color="primary" dark>
        <v-toolbar-title>Schafkopf Spiel</v-toolbar-title>
      </v-toolbar>
      <v-row justify="center">
        <v-col cols="12" sm="8" md="6">
          <schafkopf-game />
        </v-col>
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
  margin-top: 60px;
}
</style>

