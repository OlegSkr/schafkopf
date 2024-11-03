<template>
  <v-container>
    <v-row justify="space-between" align="center">
      <v-col cols="2" class="text-center">
        <v-card>
          <v-card-title class="text-h6">{{ players[0].name }}</v-card-title>
          <v-card-text>
            <v-row>
              <v-col v-for="(card, cardIndex) in players[0].hand" :key="cardIndex" cols="6">
                <v-btn @click="playCard(card)" class="card-button">
                  {{ card.rank }} {{ card.suit }}
                </v-btn>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>

      <v-col cols="8" class="text-center">
        <v-card>
          <v-card-title class="text-h6">{{ players[1].name }}</v-card-title>
          <v-card-text>
            <v-row>
              <v-col v-for="(card, cardIndex) in players[1].hand" :key="cardIndex" cols="4">
                <v-btn @click="playCard(card)" class="card-button">
                  {{ card.rank }} {{ card.suit }}
                </v-btn>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>

      <v-col cols="2" class="text-center">
        <v-card>
          <v-card-title class="text-h6">{{ players[2].name }}</v-card-title>
          <v-card-text>
            <v-row>
              <v-col v-for="(card, cardIndex) in players[2].hand" :key="cardIndex" cols="6">
                <v-btn @click="playCard(card)" class="card-button">
                  {{ card.rank }} {{ card.suit }}
                </v-btn>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <v-row justify="center">
      <v-col cols="12" class="text-center">
        <v-card>
          <v-card-title class="text-h6">{{ players[3].name }}</v-card-title>
          <v-card-text>
            <v-row>
              <v-col v-for="(card, cardIndex) in players[3].hand" :key="cardIndex" cols="4">
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
</template>

<script>
export default {
  name: 'SchafkopfGame',
  data() {
    return {
      players: [
        { name: 'Player 1', hand: [] },
        { name: 'Player 2', hand: [] },
        { name: 'Player 3', hand: [] },
        { name: 'You', hand: [] }, // Основной игрок
      ],
      deck: this.createDeck(),
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
      console.log('Played card:', card);
      // Логика обработки сыгранной карты
    },
  },
  mounted() {
    this.dealCards();
  },
};
</script>

<style scoped>
.card-button {
  width: 100%;
  margin: 5px 0;
}
</style>
