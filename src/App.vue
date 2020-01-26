<template lang="pug">
  v-app
    v-app-bar(
      app
      color="primary"
      dark
      flat
    )
      h1
        v-icon(style="font-size: 45px;").mb-1 mdi-badminton
        span.ml-3 Round {{round}}
      v-spacer
      v-btn(@click="setFullscreen").mr-2 Fullscreen
      v-btn(@click="resetRound(false)").mr-2 Reset Round
      v-btn(@click="resetGame(false)") Reset Game
    v-content#fullscreen.white
      v-container(fuild class="fill-height")
        v-row(
          align="center"
          justify="center"
        )
          v-col(cols="12" sm="12" md="12")
            v-spacer
            v-btn(v-if="isFullscreen" @click="exitFullscreen")
              v-icon mdi-fullscreen-exit
          v-col(cols="12" sm="6" md="6")
            h1 Player 1
            v-card(height="100%" @click="increment('player1')")
              v-card-actions
                h2.grey--text Win Streak
                rating(
                  :win-streak="roundsScore.player1"
                )
              v-card-text(style="height: 300px;").text-center
                h1.player-score {{scores.player1}}
              v-card-actions
                v-btn(@click.stop="decrement('player1')" large) -1
          v-col(cols="12" sm="6" md="6")
            h1 Player 2
            v-card(height="100%" @click="increment('player2')")
              v-card-actions
                h2.grey--text Win Streak
                rating(
                  :win-streak="roundsScore.player2"
                )
              v-card-text(style="height: 300px;").text-center
                h1.player-score {{scores.player2}}
              v-card-actions
                v-btn(@click.stop="decrement('player2')" large) -1

    v-dialog(v-model="roundDialog" width="400" persistent)
      v-card
        v-card-text.pa-5.text-center
          h1.display-2 {{winner | start-case}} Wins Round {{round}}!
        v-card-actions
          v-btn(block large color="success" @click="startNextRound").white--text Start Next Round&nbsp;
            v-icon mdi-heart

    v-dialog(v-model="winnerDialog" width="700" persistent)
      v-card
        v-img(src="./assets/game-winner.png")
        v-card-text.pa-5.text-center
          h1.display-2 {{winner | start-case}} Win the Game!
        v-card-actions
          v-btn(block large color="primary" @click="resetGame(true)").white--text Start Next Game&nbsp;
            v-icon mdi-heart

    v-footer(
      bottom
    )
      v-spacer
      v-btn(
        target="_blank"
        href="https://github.com/jofftiquez/badminton-scorer"
      ).black--text Made with&nbsp;
        v-icon.red--text mdi-heart
        | jofftiquez
</template>

<script>
import Rating from '@/components/rating';
import _ from 'lodash';
const WINNING_SCORE = 21;

export default {
  name: 'App',
  components: {
    Rating
  },
  filters: {
    startCase (str) {
      return _.startCase(str);
    }
  },
  data () {
    return {
      roundDialog: false,
      winnerDialog: false,
      winner: 'player1',
      scores: {
        player1: 0,
        player2: 0
      },
      round: 1,
      roundsScore: {
        player1: 0,
        player2: 0
      },
      isFullscreen: false
    };
  },
  methods: {
    increment (player) {
      this.scores[player] += 1;
      if (this.scores[player] >= WINNING_SCORE) {
        this.manageWinner(player);
      }
    },
    decrement (player) {
      if (this.scores[player] === 0) return;
      this.scores[player] -= 1;
    },
    manageWinner (player) {
      this.winner = player;
      this.roundsScore[player] += 1;
      if (this.roundsScore[player] === 2) {
        this.endGame(player);
        return;
      }
      this.roundDialog = true;
    },
    startNextRound () {
      this.roundDialog = false;
      this.resetRound(true);
      this.round += 1;
    },
    endGame (player) {
      this.winnerDialog = true;
    },
    async resetRound (force) {
      let that = this;
      const reset = () => {
        that.scores = {
          player1: 0,
          player2: 0
        };
      };

      if (force) {
        reset();
      } else {
        if (await confirm('Do you really want to reset this round?')) {
          reset();
        };
      }
    },
    async resetGame (force) {
      let that = this;
      const reset = () => {
        that.round = 1;
        that.roundsScore = {
          player1: 0,
          player2: 0
        };
        that.scores = {
          player1: 0,
          player2: 0
        };
        that.winnerDialog = false;
      };

      if (force) {
        reset();
      } else {
        if (await confirm('Do you really want to reset the game?')) {
          reset();
        };
      }
    },
    setFullscreen () {
      const elem = document.getElementById('fullscreen');
      if (elem.requestFullscreen) {
        elem.requestFullscreen();
      } else if (elem.mozRequestFullScreen) { /* Firefox */
        elem.mozRequestFullScreen();
      } else if (elem.webkitRequestFullscreen) { /* Chrome, Safari and Opera */
        elem.webkitRequestFullscreen();
      } else if (elem.msRequestFullscreen) { /* IE/Edge */
        elem.msRequestFullscreen();
      }
      this.isFullscreen = true;
    },
    exitFullscreen () {
      document.exitFullscreen();
      this.isFullscreen = false;
    }
  },
  created () {
    // this.$vuetify.theme.dark = true;
  }
};
</script>

<style scoped>
.player-score {
  font-size: 250px;
  margin-top: 130px;
}
</style>
