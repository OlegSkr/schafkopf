<template>
  <v-container fluid class="game-board">
    <!-- Grid layout for player positions -->
    <div class="grid-container">
      <!-- Player positions -->
      <div class="grid-item" id="links">
        <v-card>
          <v-card-title>{{ players[0].name }}</v-card-title>
          <v-card-text>
            <div class="card-row">
              <v-img
                v-for="(card, cardIndex) in players[0].hand"
                :key="cardIndex"
                :src="require(`@/assets/cards/${card.rank}_${card.suit}.png`)"
                class="card-image"
              />
            </div>
            <div class="response-text">
              {{ responses[0] }} <!-- ChatGPT response for player 1 -->
            </div>
          </v-card-text>
        </v-card>
      </div>

      <div class="grid-item" id="oben">
        <v-card>
          <v-card-title>{{ players[1].name }}</v-card-title>
          <v-card-text>
            <div class="card-row">
              <v-img
                v-for="(card, cardIndex) in players[1].hand"
                :key="cardIndex"
                :src="require(`@/assets/cards/${card.rank}_${card.suit}.png`)"
                class="card-image"
              />
            </div>
            <div class="response-text">
              {{ responses[1] }} <!-- ChatGPT response for player 2 -->
            </div>
          </v-card-text>
        </v-card>
      </div>

      <!-- Button in the center cell -->
      <div class="grid-item" id="center">
        <v-btn @click="askChatGPTForAllPlayers" color="primary">ChatGPT fragen</v-btn>
      </div>

      <div class="grid-item" id="rechts">
        <v-card>
          <v-card-title>{{ players[2].name }}</v-card-title>
          <v-card-text>
            <div class="card-row">
              <v-img
                v-for="(card, cardIndex) in players[2].hand"
                :key="cardIndex"
                :src="require(`@/assets/cards/${card.rank}_${card.suit}.png`)"
                class="card-image"
              />
            </div>
            <div class="response-text">
              {{ responses[2] }} <!-- ChatGPT response for player 3 -->
            </div>
          </v-card-text>
        </v-card>
      </div>

      <div class="grid-item" id="du">
        <v-card>
          <v-card-title>{{ players[3].name }}</v-card-title>
          <v-card-text>
            <div class="card-row">
              <v-img
                v-for="(card, cardIndex) in players[3].hand"
                :key="cardIndex"
                :src="require(`@/assets/cards/${card.rank}_${card.suit}.png`)"
                class="card-image"
              />
            </div>
            <div class="response-text">
              {{ responses[3] }} <!-- ChatGPT response for player "You" -->
            </div>
          </v-card-text>
        </v-card>
      </div>
    </div>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'SchafkopfGame',
  data() {
    return {
      responses: ['', '', '', ''], // ChatGPT responses for each player
      players: [
        { name: 'Links', hand: [] },
        { name: 'Oben', hand: [] },
        { name: 'Rechts', hand: [] },
        { name: 'Du', hand: [] },
      ],
      deck: this.createDeck(),
    };
  },
  methods: {
    // Creates a deck of cards
    createDeck() {
      const suits = ['Eichel', 'Schellen', 'Grün', 'Herz'];
      const ranks = ['7', '8', '9', '10', 'Unter', 'Ober', 'König', 'Ass'];
      let deck = [];
      suits.forEach(suit => {
        ranks.forEach(rank => {
          deck.push({ rank, suit });
        });
      });
      return deck;
    },

    // Shuffles the deck using the Fisher-Yates algorithm
    shuffleDeck(deck) {
      for (let i = deck.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [deck[i], deck[j]] = [deck[j], deck[i]];
      }
    },

    // Deals cards to each player
    dealCards() {
      this.shuffleDeck(this.deck);
      this.players.forEach(player => {
        player.hand = this.deck.splice(0, 8);
      });
    },

    // Sends a ChatGPT query for each player
    async askChatGPTForAllPlayers() {
      for (let i = 0; i < this.players.length; i++) {
        const player = this.players[i];
        // Prompt in German for ChatGPT
        const prompt = `Neues Spiel, Spieler ${player.name} hat die Karten: ${player.hand.map(card => `${card.rank} ${card.suit}`).join(', ')}. Soll er spielen?`;

        try {
          const response = await axios.post('https://api.openai.com/v1/chat/completions', {
            model: 'gpt-3.5-turbo',
            messages: [{ role: 'user', content: prompt }],
          }, {
            headers: {
              'Authorization': `Bearer ${process.env.VUE_APP_OPENAI_API_KEY}`,
              'Content-Type': 'application/json',
            },
          });

          // Directly assigning response for each player
          this.responses[i] = response.data.choices[0].message.content;
        } catch (error) {
          console.error(`Error requesting response for ${player.name}:`, error);
          this.responses[i] = 'Fehler beim Abrufen der Antwort.'; // Error message in German
        }
      }
    },
  },
  mounted() {
    this.dealCards();
  },
};
</script>

<style scoped>
.game-board {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  height: auto;
  margin: 0;
}

.grid-container {
  display: grid;
  grid-template-rows: repeat(3, 1fr);
  grid-template-columns: repeat(3, 1fr);
  width: 100%;
  height: auto;
  padding: 0;
  box-sizing: border-box;
}

.grid-item {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

#links { grid-row: 2; grid-column: 1; }
#oben { grid-row: 1; grid-column: 2; }
#rechts { grid-row: 2; grid-column: 3; }
#du { grid-row: 3; grid-column: 2; }
#center {
  grid-row: 2;
  grid-column: 2;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card-row {
  display: flex;
  flex-direction: row;
  overflow-x: auto;
}

.card-image {
  width: 50px;
  height: auto;
  margin-right: 5px;
}

.response-text {
  margin-top: 8px;
  text-align: center;
}
</style>
