<template>
  <div class="player">
    <h3>{{ player.name }}</h3>
    <div class="hand">
      <PlayingCard
        v-for="(card, index) in player.hand"
        :key="index"
        :card="card"
        @play="onCardPlay"
      />
    </div>
  </div>
</template>

<script>
import PlayingCard from './PlayingCard.vue';

export default {
  name: 'GamePlayer', // Обновите имя компонента
  components: {
    PlayingCard,
  },
  props: {
    player: Object,
  },
  methods: {
    onCardPlay(card) {
      this.$emit('card-played', card);
    },
  },
};
</script>

<style scoped>
.player {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 10px;
}
.hand {
  display: flex;
}
</style>

