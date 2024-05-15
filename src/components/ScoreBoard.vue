<template>
  <div class="scoreboard-outer">
    <div class="round">Round {{ gameInfo.currentRound }}</div>
    <div class="scoreboard">
      <div class="score-column caption-win">
        <div class="score-header">Player</div>
        <div class="score">{{ gameInfo.playerScore }}</div>
      </div>
      <div class="score-column caption-lose">
        <div class="score-header">Computer</div>
        <div class="score">{{ gameInfo.computerScore }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["gameInfo"],
};
</script>

<style scoped>
.scoreboard-outer {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.scoreboard {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: auto;
}
.score-column {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 1rem;
  font-weight: bold;
  width: 10rem;
}
.score-header {
  text-transform: uppercase;
  font-size: 1.8rem;
}
.score {
  font-size: 1.6rem;
}

.caption-lose {
  color: #d82130;
  filter: drop-shadow(1px 1px 3px var(--black));
}

.caption-win {
  color: #21d84f;
  filter: drop-shadow(1px 1px 3px var(--black));
}
</style>
