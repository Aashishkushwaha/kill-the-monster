<template>
  <div>
    <Modal
      :show="winner"
      :modalText="modalText"
      :confirmHandler="() => restartGame(true)"
      :cancelHandler="() => restartGame(false)"
    />
    <header class="header">
      <div class="header-player-box">
        <div class="header-player-sub-box">
          <h3 class="player-label">You</h3>
          <div>
            <span class="heart" :key="index" v-for="(heal, index) in remainingHeals">{{heal}}</span>
            <span class="heart" :key="'heal'+index" v-for="(heal, index) in healsUsed">{{heal}}</span>
          </div>
        </div>
        <div class="progress-track">
          <div class="progress" :style="{width: renderPlayerHealth}">
            <span v-if="playerHealth > 6">{{renderPlayerHealth}}</span>
          </div>
        </div>
      </div>
      <h4>Vs</h4>
      <div class="header-player-box">
        <h3>Monster</h3>
        <div class="progress-track">
          <div class="progress" :style="{width: renderMonsterHealth}">
            <span v-if="monsterHealth > 6">{{renderMonsterHealth}}</span>
          </div>
        </div>
      </div>
    </header>
    <main>
      <div class="btn-container">
        <template v-if="!gameStarted">
          <button class="btn btn-rounded-full btn-teal" v-on:click="startStopGame">Start Game</button>
        </template>
        <template v-if="gameStarted">
          <button class="btn btn-rounded btn-green" @click="attack">Normal Attack</button>
          <button class="btn btn-rounded btn-orange" @click="specialAttack">Special Attack</button>
          <button class="btn btn-rounded btn-teal" @click="heal">Heal</button>
          <button class="btn btn-rounded btn-red" @click="startStopGame">Quit</button>
        </template>
      </div>
      <div class="game-container" v-if="gameHistory.length">
        <div>
          <div
            v-for="(result, index) in gameHistory"
            :key="index"
            :class="'game-results-row'"
            :style="{background: result.playerHit ? 'dodgerblue' : 'tomato'}"
          >{{result.text}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Modal from "./Modal.vue";

export default {
  name: "Game",
  data: function () {
    return {
      title: "Hello Vue.js",
      monsterHealth: 100,
      playerHealth: 100,
      gameStarted: false,
      gameHistory: [],
      winner: "",
      remainingHeals: ["❤️", "❤️", "❤️"],
      healsUsed: [],
      modalText: "Result will appear here...",
    };
  },
  template: `
    <div>{{monsterHealth}}</div>
  `,
  methods: {
    startStopGame: function () {
      this.gameStarted = !this.gameStarted;
    },
    restartGame: function (restart) {
      document.body.style.overflow = "auto";
      this.playerHealth = this.monsterHealth = 100;
      this.gameHistory = [];
      this.gameStarted = restart;
      this.winner = "";
      this.remainingHeals = ["❤️", "❤️", "❤️"];
      this.healsUsed = [];
      this.modalText = "Result will appear here...";
    },
    showWinner: function (callbackFuntion) {
      document.body.style.overflow = "hidden";
      setTimeout(callbackFuntion, 400);
    },
    attack: function () {
      let playerHit = Math.floor(Math.random() * 11);
      let monsterHit = Math.floor(Math.random() * 11);
      if (this.monsterHealth < playerHit) {
        playerHit = this.monsterHealth;
      }
      if (this.playerHealth < monsterHit) {
        monsterHit = this.playerHealth;
      }
      this.gameHistory = [
        {
          text: `Player hits monster for ${playerHit} XP.`,
          playerHit: true,
        },
        {
          text: `Monster hits player for ${monsterHit} XP.`,
          playerHit: false,
        },
        ...this.gameHistory,
      ];
      this.playerHealth -= monsterHit;
      this.monsterHealth -= playerHit;
    },
    specialAttack: function () {
      let playerHit = Math.floor(Math.random() * 16);
      let monsterHit = Math.floor(Math.random() * 11);
      if (this.monsterHealth < playerHit) {
        playerHit = this.monsterHealth;
      }
      if (this.playerHealth < monsterHit) {
        monsterHit = this.playerHealth;
      }
      this.gameHistory = [
        {
          text: `Player hits monster for ${playerHit} XP.`,
          playerHit: true,
        },
        {
          text: `Monster hits player for ${monsterHit} XP.`,
          playerHit: false,
        },
        ...this.gameHistory,
      ];
      this.playerHealth -= monsterHit;
      this.monsterHealth -= playerHit;
    },
    heal: function () {
      if (
        this.playerHealth <= 95 &&
        this.remainingHeals &&
        this.healsUsed.length < 3
      ) {
        this.playerHealth += 5;
        this.healsUsed.push("💔");
        this.remainingHeals.pop();
      }
    },
  },
  computed: {
    renderPlayerHealth: function () {
      return this.playerHealth + "%";
    },
    renderMonsterHealth: function () {
      return this.monsterHealth + "%";
    },
  },
  watch: {
    gameStarted: function (value) {
      if (!value) {
        this.restartGame(value);
      }
    },
    winner: function (value) {
      if (value === "player") {
        this.showWinner(() => {
          this.modalText = "You won 🤩 !!! want to play again?";
        });
      } else if (value === "monster") {
        this.showWinner(() => {
          this.modalText = "You Lost 😞 !!! want to play again?";
        });
      } else if (value === "none") {
        this.showWinner(() => {
          this.modalText = "It's a tie 🙂 !!! want to play again?";
        });
      }
    },
    playerHealth: function (value) {
      if (value > 0 && this.monsterHealth === 0) {
        this.winner = "player";
      } else if (value === 0 && this.monsterHealth === 0) {
        this.winner = "none";
      }
    },
    monsterHealth: function (value) {
      if (value > 0 && this.playerHealth === 0) {
        this.winner = "monster";
      } else if (value === 0 && this.playerHealth === 0) {
        this.winner = "none";
      }
    },
  },
  components: {
    Modal,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: center;
  padding: 0.5rem 0;
}

.header h3,
h4 {
  margin: 8px;
}

.header-player-box {
  display: flex;
  flex-direction: column;
  width: 45%;
}

.header-player-sub-box {
  display: flex;
  align-items: center;
}

.header-player-sub-box > div {
  display: flex;
}

.heart {
  margin: 0 1px;
}

.player-label {
  width: 65%;
  padding-left: 4rem;
}

.progress-track {
  width: 100%;
  height: 25px;
  background: #e9e9e9;
  border-radius: 50px;
}

.progress {
  height: 100%;
  width: 35%;
  background: green;
  border-radius: 50px;
  box-shadow: 0 0 0.2rem -1px green;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  transition: width 0.3s;
}

.btn-container {
  padding: 1rem 0.5rem;
  margin: 0.4rem 0;
  border-radius: 4px;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #e1e1e1;
  flex-wrap: wrap;
}

.game-container {
  margin: 0.5rem auto;
  padding: 0.5rem;
  background: #f3f3f3;
  border-radius: 4px;
  max-height: 400px;
  overflow: hidden;
  overflow-y: auto;
}

.game-results-row {
  padding: 0.4rem;
  margin: 0.2rem 0;
  width: 100%;
  color: white;
  text-align: center;
  border-radius: 4px;
}

::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-track {
  background: #e1e1e1;
  border-radius: 50px;
}

::-webkit-scrollbar-thumb {
  background: #c1c1c1;
  border-radius: 50px;
}

@media screen and (max-width: 600px) {
  .app-title {
    font-size: 10px;
  }

  .header {
    flex-direction: column;
  }

  .header-player-box {
    width: 100%;
  }

  .header-player-box:last-child > h3 {
    order: 2;
  }
}
</style>

<style>
.btn {
  margin: 4px;
  padding: 12px 8px;
  width: 110px;
  border: none;
  outline: none;
  color: white;
  cursor: pointer;
  box-shadow: 0 2px 4px -3px black, 2px 0 4px -3px black, 0 -2px 4px -3px black,
    -2px 0 4px -3px black;
  transform: translateY(-2px);
}

.btn-rounded {
  border-radius: 4px;
}

.btn-rounded-full {
  border-radius: 50px;
}

.btn-red {
  background: red;
}

.btn-orange {
  background: orange;
}

.btn-green {
  background: green;
}

.btn-teal {
  background: teal;
}
</style>