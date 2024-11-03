<template>
  <v-app>
    <v-container>
      <v-row class="justify-center">
        <v-col cols="12" class="text-center">
          <v-toolbar>
            <v-toolbar-title>Schafkopf Spiel</v-toolbar-title>
          </v-toolbar>
        </v-col>
      </v-row>
      <v-row>
        <v-col v-for="(player, index) in players" :key="index" cols="3" class="player-column">
          <v-card>
            <v-card-title class="text-h6">{{ player.name }}</v-card-title>
            <v-card-text>
              <v-row>
                <v-col v-for="(card, cardIndex) in player.hand" :key="cardIndex" cols="6">
                  <v-btn @click="playCard(card)" class="card-button">
                    {{ card.rank }} {{ card.suit }}
                  </v-btn>
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
// import GamePlayer from './GamePlayer.vue';

export default {
  name: 'SchafkopfGame',
/*
  components: {
    GamePlayer,
  },
*/
  data() {
    return {
      players: [
        { name: 'Spieler 1', hand: [] },
        { name: 'Spieler 2', hand: [] },
        { name: 'Spieler 3', hand: [] },
        { name: 'Spieler 4', hand: [] },
      ],
      deck: this.createDeck(),
      trumpSuit: 'Eichel',
      playerPoints: [0, 0, 0, 0],
    };
  },
  methods: {
    createDeck() {
      const suits = ['Eichel', 'Schellen', 'Grün', 'Herz'];
      const ranks = ['7', '8', '9', '10', 'Bube', 'Dame', 'König', 'Ass'];
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
        [deck[i], deck[j]] = [deck[j], deck[i]];
      }
    },

    dealCards() {
      this.shuffleDeck(this.deck);
      this.players.forEach(player => {
        player.hand = this.deck.splice(0, 8);
      });
    },

    playCard(card) {
      console.log('Karte gespielt:', card);
      // Логика обработки сыгранной карты
    },
  },
  mounted() {
    this.dealCards();
  },
};
</script>

<style scoped>
.player-column {
  margin-bottom: 20px;
}

.card-button {
  width: 100%;
  margin: 5px 0;
}
</style>
