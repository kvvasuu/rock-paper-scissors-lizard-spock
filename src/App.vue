<template>
  <welcome-page v-if="!playPageShow" @play-game="startGame"></welcome-page>

  <transition name="bounce">
    <div v-if="playPageShow" class="content">
      <score-board :game-info="gameInfo"></score-board>
      <div class="play-area">
        <div class="battle-area">
          <fight-component
            :isFightOver="isFightOver"
            :playerMove="playerMove"
            :computerMove="computerMove"
            :lastRoundResult="lastRoundResult"
            :isGameOver="isGameOver"
            :winner="winner"
            :fightSymbol="fightSymbol"
          ></fight-component>
        </div>
        <transition name="fade">
          <div v-if="!hideMoves">
            <span>Choose move</span>
            <div class="available-moves">
              <div
                v-for="move in moves"
                :title="move.title"
                :key="move.title"
                @click="fight(move.title)"
                class="move"
              >
                <div class="move-icon">
                  <i :class="move.icon"></i>
                </div>
                <p class="move-caption">{{ move.title }}</p>
              </div>
            </div>
          </div>
        </transition>
        <transition name="bounce">
          <div v-if="isGameOver">
            <button @click="resetGame" class="button-play">Try again</button>
          </div>
        </transition>
      </div>
    </div>
  </transition>
</template>

<script>
import ScoreBoard from "./components/ScoreBoard.vue";
import WelcomePage from "./components/WelcomePage.vue";
import FightComponent from "./components/FightComponent.vue";

export default {
  components: {
    ScoreBoard,
    WelcomePage,
    FightComponent,
  },
  data() {
    return {
      gameInfo: {
        playerScore: 0,
        computerScore: 0,
        currentRound: 1,
        numberOfRounds: 1,
      },
      lastRoundResult: "",
      winner: "",
      isGameOver: false,
      hideMoves: false,
      isFightOver: false,
      playPageShow: false,
      playerMove: null,
      computerMove: null,
      fightSymbol: "",
      winPossibilities: [
        "rs",
        "sp",
        "pr",
        "rl",
        "lk",
        "ks",
        "sl",
        "lp",
        "pk",
        "kr",
      ],
      moves: {
        rock: {
          title: "Rock",
          icon: "fa-regular fa-hand-back-fist",
          symbol: "r",
        },
        paper: {
          title: "Paper",
          icon: "fa-regular fa-hand",
          symbol: "p",
        },
        scissors: {
          title: "Scissors",
          icon: "fa-regular fa-hand-scissors",
          symbol: "s",
        },
        lizard: {
          title: "Lizard",
          icon: "fa-regular fa-hand-lizard",
          symbol: "l",
        },
        spock: {
          title: "Spock",
          icon: "fa-regular fa-hand-spock",
          symbol: "k",
        },
      },
    };
  },
  watch: {
    "gameInfo.playerScore": function (newVal, oldVal) {
      if (newVal >= this.gameInfo.numberOfRounds) {
        this.winner = "Player";
        this.hideMoves = true;
      }
    },
    "gameInfo.computerScore": function (newVal, oldVal) {
      if (newVal >= this.gameInfo.numberOfRounds) {
        this.winner = "Computer";
        this.hideMoves = true;
      }
    },
  },
  methods: {
    startGame(rounds) {
      this.gameInfo.numberOfRounds = rounds;
      this.playPageShow = true;
    },
    async fight(move) {
      this.hideMoves = true;
      this.playerMove = this.moves[move.toLowerCase()];
      this.getComputerMove();

      setTimeout(() => {
        this.resolve();
        this.isFightOver = true;
      }, 1000);

      setTimeout(() => {
        this.isFightOver = false;
      }, 3000);

      setTimeout(() => {
        if (
          this.gameInfo.computerScore >= this.gameInfo.numberOfRounds ||
          this.gameInfo.playerScore >= this.gameInfo.numberOfRounds
        ) {
          this.isGameOver = true;
        } else {
          this.gameInfo.currentRound++;
          this.hideMoves = false;
        }
      }, 4000);
    },
    getComputerMove() {
      const moveNumber = Math.floor(Math.random() * (6 - 1) + 1) - 1;
      this.computerMove = this.moves[Object.keys(this.moves)[moveNumber]];
    },
    resolve() {
      this.fightSymbol = `${this.playerMove.symbol}${this.computerMove.symbol}`;
      if (this.playerMove.title === this.computerMove.title) {
        this.lastRoundResult = "Draw";
      } else if (this.winPossibilities.includes(this.fightSymbol)) {
        this.lastRoundResult = "Win";
        this.gameInfo.playerScore++;
      } else {
        this.lastRoundResult = "Lose";
        this.gameInfo.computerScore++;
      }
    },
    resetGame() {
      (this.gameInfo = {
        playerScore: 0,
        computerScore: 0,
        currentRound: 1,
        numberOfRounds: 1,
      }),
        (this.lastRoundResult = ""),
        (this.winner = ""),
        (this.isGameOver = false),
        (this.hideMoves = false),
        (this.isFightOver = false),
        (this.playPageShow = false),
        (this.playerMove = null),
        (this.computerMove = null);
    },
  },
};
</script>

<style scoped>
.content {
  width: 100%;
  height: 50rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}

.battle-area {
  height: 30rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.available-moves {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}

.move {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  transition: all 0.2s ease-in-out;
  cursor: pointer;
  margin: 1rem;
}

.move-icon {
  font-size: 3rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10rem;
  width: 4rem;
  height: 4rem;
  border: 0.4rem solid #d9eb3ac0;
  padding: 1rem;
  color: #d9eb3ac0;
  transition: all 0.2s ease-in-out;
  cursor: pointer;
}

.move-caption {
  color: #d9eb3ac0;
  text-transform: uppercase;
  font-weight: bold;
  letter-spacing: 2px;
}

.move:hover {
  scale: 1.06;
  filter: drop-shadow(0 0 1px #d9eb3ac0);
}

.bounce-enter-active {
  animation: bounce-in 0.5s;
}

.bounce-small-enter-active {
  animation: bounce-in 0.5s;
}

.bounce-small-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

button {
  background: #26d3c7;
  border-radius: 999px;
  box-shadow: #26d3c7 1px 2px 20px -10px;
  box-sizing: border-box;
  color: #212121;
  cursor: pointer;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  outline: 0 solid transparent;
  touch-action: manipulation;
  border: 0;
  margin: 1rem;
  padding: 0.6rem;
}

.button-play {
  font-size: 1.8rem;
  padding: 1.4rem;
  margin: 3rem;
}

@media (max-width: 720px) {
  .battle-area {
    height: 20rem;
  }
}
</style>
