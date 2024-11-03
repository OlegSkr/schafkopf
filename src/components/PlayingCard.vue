<template>
  <div class="card" @click="playCard">
    <span>{{ card.rank }} von {{ card.suit }}</span>
  </div>
</template>

<script>
export default {
  name: 'PlayingCard', // Обновите имя компонента
  props: {
    card: Object,
  },
  methods: {
    playCard() {
      this.$emit('play', this.card);
    },
  },
  mounted() {
    console.log('Card mounted:', this.card);
  },
};
</script>

<style scoped>
.card {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 5px;
  cursor: pointer;
}
</style>
