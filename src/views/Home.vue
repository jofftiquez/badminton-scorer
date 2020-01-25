<template lang="pug">
  v-container(fuild class="fill-height")
    v-row(
      align="center"
      justify="center"
    )
      v-col(cols="12" sm="6" md="6").text-center
        v-card(height="100%" @click="increment('player1')")
          v-card-actions
            h2 Round won: {{rounds.player1}}
          v-card-text(style="height: 400px;")
            h1.player-score {{scores.player1}}
          v-card-actions
            v-btn(@click.stop="decrement('player1')" large color="warning") -1
      v-col(cols="12" sm="6" md="6").text-center
        v-card(height="100%" @click="increment('player2')")
          v-card-actions
            h2 Round won: {{rounds.player2}}
          v-card-text(style="height: 400px;")
            h1.player-score {{scores.player2}}
          v-card-actions
            v-btn(@click.stop="decrement('player2')" large color="warning") -1
</template>

<script>
export default {
  data () {
    return {
      scores: {
        player1: 0,
        player2: 0
      },
      round: 1,
      rounds: {
        player1: 0,
        player2: 0
      }
    };
  },
  methods: {
    increment (player) {
      this.scores[player] += 1;
      if (this.scores[player] === 21) {
        this.rounds[player] += 1;
        this.resetRound();
      }
    },
    decrement (player) {
      if (this.scores[player] === 0) return;
      this.scores[player] -= 1;
    },
    resetRound () {
      this.scores = {
        player1: 0,
        player2: 0
      };
    }
  }
};
</script>

<style scoped>
.player-score {
  font-size: 250px;
  margin-top: 150px;
}
</style>
