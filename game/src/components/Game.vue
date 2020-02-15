<template>
  <div class="container">
    <section class="players d-flex justify-content-around mb-3">
      <div class="player">
        <h4 class="player-name d-flex flex-column align-items-center">
          YOU
          <img :src="playerImg" width="150px" alt="YOU" />
        </h4>
        <div class="health-bar">
          <p class="healthRemaining">{{ playerHealth }}</p>
          <div
            class="health-left"
            :style="{ width: playerHealth > 0 ? playerHealth + '%' : 0 + '%' }"
          ></div>
          <div class="health-empty"></div>
        </div>
      </div>
      <div class="player">
        <h4 class="player-name d-flex flex-column align-items-center">
          MONSTER
          <img :src="monsterImg" width="141px" alt="Monster" class="flip" />
        </h4>
        <div class="health-bar">
          <p class="healthRemaining">{{ monsterHealth }}</p>
          <div
            class="health-left"
            :style="{
              width: monsterHealth > 0 ? monsterHealth + '%' : 0 + '%'
            }"
          ></div>
          <div class="health-empty"></div>
        </div>
      </div>
    </section>
    <b-card class="text-center mb-3 d-flex">
      <b-button v-if="!gameIsRunning" @click="startGame"
        >Start New Game</b-button
      >
      <div v-else>
        <b-button variant="danger" @click="attack" :disabled="btnDisabled"
          >Attack</b-button
        >
        <b-button
          class="ml-2"
          variant="warning"
          @click="specialAttack"
          :disabled="btnDisabled"
          >Special Attack</b-button
        >
        <b-button
          class="ml-2"
          variant="success"
          @click="heal"
          :disabled="btnDisabled"
          >Heal</b-button
        >
        <b-button class="ml-2" @click="giveUp" :disabled="btnDisabled"
          >Give Up</b-button
        >
      </div>
    </b-card>
    <b-card class="text-center">
      <div
        class="mb-1"
        :style="{
          fontWeight: 900,
          backgroundColor: `${(player =
            turn[0] == 1 ? 'rgb(150, 203, 255)' : 'rgb(255, 153, 161)')}`,
          color: `${(player = turn[0] == 1 ? 'blue' : 'red')}`
        }"
        v-for="(turn, index) in turns"
        :key="index"
      >
        {{ (player = turn[0] == 1 ? "Player" : "Monster") }} hits for
        {{ turn[1] }}
      </div>
    </b-card>
  </div>
</template>

<script>
import playerImg from "/home/jlublinskis/hello-world/guilty-gear-ky-gif-6.gif";
import monsterImg from "/home/jlublinskis/hello-world/gif-blanka-street-fighter-8.gif";
export default {
  name: "Game",
  props: {
    msg: String
  },
  data() {
    return {
      playerImg: playerImg,
      playerHealth: 100,
      monsterImg: monsterImg,
      monsterHealth: 100,
      gameIsRunning: false,
      btnDisabled: false,
      player: "",
      turns: [],
      power: 0
    };
  },
  methods: {
    startGame() {
      (this.playerHealth = 100),
        (this.monsterHealth = 100),
        (this.turns = []),
        (this.gameIsRunning = true),
        (this.btnDisabled = false);
    },
    attack() {
      this.monsterAttack();
      this.power = this.calculateDamage(1, 10);
      this.turns.unshift([1, this.power]);
      this.monsterHealth - this.power <= 0
        ? (this.monsterHealth = 0)
        : (this.monsterHealth -= this.power);
      return this.checkWin();
    },
    specialAttack() {
      this.monsterAttack();
      this.power = this.calculateDamage(10, 10);
      this.turns.unshift([1, this.power]);
      this.monsterHealth - this.power <= 0
        ? (this.monsterHealth = 0)
        : (this.monsterHealth -= this.power);
      return this.checkWin();
    },
    heal() {
      this.monsterAttack();
      this.power = this.calculateDamage(5, 10);
      this.turns.unshift([1, this.power]);
      this.playerHealth + this.power > 100
        ? (this.playerHealth = 100)
        : (this.playerHealth += this.power);
      return this.checkWin();
    },
    giveUp: function() {
      this.gameIsRunning = false;
    },
    monsterAttack() {
      this.power = this.calculateDamage(1, 15);
      this.turns.unshift([0, this.power]);
      return this.playerHealth - this.power <= 0
        ? (this.playerHealth = 0)
        : (this.playerHealth -= this.power);
    },
    calculateDamage: function(min, max) {
      return Math.round(Math.random() * max) + min;
    },
    checkWin: function() {
      if (this.playerHealth <= 0 || this.monsterHealth <= 0) {
        this.btnDisabled = true;
        if (
          confirm(
            `${
              this.playerHealth > 0 ? "YOU won. " : "YOU lose. "
            }Would you like to start a new game?`
          )
        ) {
          return this.startGame();
        } else {
          return this.giveUp();
        }
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  max-width: 900px;
  width: 90%;
}

.player {
  width: 45%;
}

.health-bar {
  background-color: rgb(182, 182, 182);
  width: 100%;
  height: 40px;
  position: relative;
}

.health-left {
  background-color: green;
  max-width: 100%;
  height: 40px;
}

.health-empty {
  background-color: transparent;
  height: 40px;
}

.healthRemaining {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.flip {
  transform: scaleX(-1);
}
</style>
