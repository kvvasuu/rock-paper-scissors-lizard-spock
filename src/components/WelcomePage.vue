<template>
  <div>
    <div class="header">
      <h1>Rock Paper Scissors Lizard Spock</h1>
    </div>
    <div class="content">
      <p>Number of points to win:</p>
      <div class="rounds-number">
        <button class="button-decrease" @click="decrease">
          <i class="fa-solid fa-minus"></i>
        </button>
        <input
          @keyup="formatNumber($event.target.value)"
          type="number"
          class="input-1"
          v-model="numberOfRounds"
          min="1"
          max="10"
        />
        <button class="button-increase" @click="increase">
          <i class="fa-solid fa-plus"></i>
        </button>
      </div>
      <button class="button-play" @click="$emit('play-game', numberOfRounds)">
        Play
      </button>
    </div>
  </div>
</template>

<script>
export default {
  emits: ["play-game"],
  data() {
    return {
      numberOfRounds: 3,
    };
  },
  methods: {
    increase() {
      if (this.numberOfRounds < 10) this.numberOfRounds++;
    },
    decrease() {
      if (this.numberOfRounds > 1) this.numberOfRounds--;
    },
    formatNumber(num) {
      if (num > 10) this.numberOfRounds = 10;
    },
  },
};
</script>

<style scoped>
.content {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.header {
  font-size: 1.6rem;
  color: #26d3c7;
  filter: drop-shadow(1px 1px 3px var(--black));
  margin: 1rem;
}

.rounds-number {
  display: flex;
  align-items: center;
  justify-content: center;
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
  height: 2rem;
  width: 5rem;
  outline: 0 solid transparent;
  touch-action: manipulation;
  border: 0;
  margin: 1rem;
  padding: 0.6rem;
}

.button-play {
  font-size: 1.8rem;
  width: 8rem;
  height: 3rem;
  margin: 3rem;
}

button:active {
  background: rgb(38, 190, 180);
}

.button-increase {
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
  margin: 0;
  width: 2.3rem;
  justify-content: end;
  padding: 0.6rem;
}

.button-increase:active {
  background: rgb(38, 211, 199);
  background: linear-gradient(
    90deg,
    rgba(38, 211, 199, 1) 0%,
    rgba(38, 190, 180, 1) 35%
  );
}

.button-decrease {
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
  margin: 0;
  width: 3rem;
  width: 2.3rem;
  justify-content: start;
  padding: 0.6rem;
}

.button-decrease:active {
  background: rgb(38, 211, 199);
  background: linear-gradient(
    -90deg,
    rgba(38, 211, 199, 1) 0%,
    rgba(38, 190, 180, 1) 35%
  );
}

.input-1 {
  background: #26d3c7;
  border-radius: 0;
  box-shadow: #26d3c7 5px 5px 20px -10px;
  box-sizing: border-box;
  color: #212121;
  font-size: 1rem;
  font-weight: 700;
  text-align: center;
  height: 2rem;
  width: 2rem;
  outline: 0 solid transparent;
  touch-action: manipulation;
  border: 0;
  margin: 0;
  overflow: visible;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
