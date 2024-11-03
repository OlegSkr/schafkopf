<template>
  <div class="game">
    <h1>Schafkopf Spiel</h1>
    <div class="players">
      <GamePlayer
        v-for="(player, index) in players"
        :key="index"
        :player="player"
        @card-played="playCard"
      />
    </div>
  </div>
</template>

<script>
import GamePlayer from './GamePlayer.vue';

export default {
  name: 'SchafkopfGame',
  components: {
    GamePlayer,
  },
  data() {
    return {
      players: [
        { name: 'Spieler 1', hand: [] },
        { name: 'Spieler 2', hand: [] },
        { name: 'Spieler 3', hand: [] },
        { name: 'Spieler 4', hand: [] },
      ],
      deck: this.createDeck(),
      trumpSuit: 'Eichel', // Определите козырную масть
      playerPoints: [0, 0, 0, 0],
    };
  },
  methods: {
    createDeck() {
      const suits = ['Eichel', 'Schellen', 'Grün', 'Herz']; // Измененные масти
      const ranks = ['7', '8', '9', '10', 'Bube', 'Dame', 'König', 'Ass']; // Новые ранги
      let deck = [];
      suits.forEach(suit => {
        ranks.forEach(rank => {
          deck.push({ rank, suit });
        });
      });
      return deck;
    },
    
    shuffleDeck(deck) {
      for (let i = deck.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [deck[i], deck[j]] = [deck[j], deck[i]]; // Перемешиваем карты
      }
    },

    dealCards() {
      this.shuffleDeck(this.deck); // Перемешиваем колоду перед раздачей
      this.players.forEach(player => {
        player.hand = this.deck.splice(0, 8); // Раздаем по 8 карт каждому игроку
      });
    },

    playCard(card) {
      console.log('Karte gespielt:', card);
      // Логика обработки сыгранной карты
    },
  },
  mounted() {
    this.dealCards(); // Раздаем карты при монтировании компонента
  },
};
</script>

<style scoped>
.game {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.players {
  display: flex;
}
</style>
