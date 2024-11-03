<template>
  <v-container fluid class="game-board">
    <!-- Сетка для размещения игроков -->
    <div class="grid-container">
      <!-- Игроки по позициям -->
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
          </v-card-text>
        </v-card>
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
          </v-card-text>
        </v-card>
      </div>
    </div>
  </v-container>
</template>

<script>
export default {
  name: 'SchafkopfGame',
  data() {
    return {
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
  flex-direction: column; /* Центрируем элементы по вертикали */
  width: 100%; /* Устанавливаем ширину на 100% */
  height: auto; /* Высота автоматически в зависимости от содержимого */
  margin: 0; /* Убираем отступы */
}

.grid-container {
  display: grid;
  grid-template-rows: repeat(3, 1fr);
  grid-template-columns: repeat(3, 1fr);
  width: 100%; /* Используем 100% ширины контейнера */
  height: auto; /* Высота автоматически в зависимости от содержимого */
  padding: 0; /* Убираем отступы */
  box-sizing: border-box; /* Учитываем отступы и границы */
}

.grid-item {
  display: flex;
  justify-content: center;
  align-items: center;
}

#links { grid-row: 2; grid-column: 1; } /* Клетка 4 */
#oben { grid-row: 1; grid-column: 2; } /* Клетка 2 */
#rechts { grid-row: 2; grid-column: 3; } /* Клетка 6 */
#du { grid-row: 3; grid-column: 2; } /* Клетка 8 */

.card-row {
  display: flex; /* Используем Flexbox для рядного расположения карт */
  flex-direction: row; /* Указываем направление по ряду */
  overflow-x: auto; /* Позволяем прокрутку по горизонтали при необходимости */
}

.card-image {
  width: 50px; /* Ширина каждой карты изменена на 50px */
  height: auto; /* Автоматическая высота изображения */
  margin-right: 5px; /* Отступ между картами */
}
</style>
