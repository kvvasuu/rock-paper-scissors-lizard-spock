<template>
  <transition name="bounce-small">
    <h1 class="winner-h1" v-if="isGameOver" :class="captionColor">
      {{ winnerCaption }}
    </h1>
    <h1 v-else-if="isFightOver" :class="captionColor">{{ caption }}</h1>
  </transition>
</template>

<script>
export default {
  props: [
    "isFightOver",
    "playerMove",
    "computerMove",
    "lastRoundResult",
    "isGameOver",
    "winner",
    "fightSymbol",
  ],
  data() {
    return {
      fightCaptions: {
        sp: "Scissors cuts Paper",
        pr: "Paper covers Rock",
        rl: "Rock crushes Lizard",
        lk: "Lizard poisons Spock",
        ks: "Spock smashes Scissors",
        sl: "Scissors decapitates Lizard",
        lp: "Lizard eats Paper",
        pk: "Paper disproves Spock",
        kr: "Spock vaporizes Rock",
        rs: "Rock crushes Scissors",
      },
      captionColor: "",
    };
  },
  computed: {
    caption() {
      if (this.lastRoundResult === "Draw") {
        this.captionColor = "caption-draw";
        return this.lastRoundResult;
      } else if (this.lastRoundResult === "Win") {
        this.captionColor = "caption-win";
        return this.fightCaptions[this.fightSymbol];
      } else {
        this.captionColor = "caption-lose";
        let fightSymbolReversed = this.fightSymbol.split("").reverse().join("");
        return this.fightCaptions[fightSymbolReversed];
      }
    },
    winnerCaption() {
      if (this.winner === "Player") {
        return "You won!";
      } else return "You lose!";
    },
  },
};
</script>

<style scoped>
.move {
  font-size: 3rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10rem;
  width: 4rem;
  height: 4rem;
  border: 0.4rem solid #d9eb3ac0;
  padding: 1rem;
  margin: 1rem;
  color: #d9eb3ac0;
  transition: all 0.2s ease-in-out;
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

.caption-lose {
  color: #d82130;
  filter: drop-shadow(1px 1px 3px var(--black));
}
.caption-win {
  color: #21d84f;
  filter: drop-shadow(1px 1px 3px var(--black));
}
.caption-draw {
  color: rgba(255, 255, 255, 0.87);
  filter: drop-shadow(1px 1px 3px var(--black));
}
</style>
