<template>

  <div class="center">

    <div class="firstdialog" v-if="isFirst">

      <strong>Хотите начать играть?</strong>

      <button @click="first">ДА</button>

    </div>

    <div class="seconddialog" v-if="isSecond">

      <input
        class="entername"
        placeholder="Введите имя"
        v-model="user.name"
        @keydown.enter="addName"
      />

      <!-- onclick="enterName()" -->

      <button class="enternamebutton" @click="addName"> OK </button>

    </div>

    <div class="maingame">

      <div class="info" v-if="isThierd">

        <div class="textOfUser">

          <p class="infotext">{{ user.name }}</p>

          <p class="infotextforhp">ХП : {{ user.HP }}</p>

          <p class="infotextfordamage">Урон : {{ user.damage }}</p>

          <div class="progress-bar-wrapper">

            <progress
              id="progressbar"
              :value="user.point"
              :max="user.scoreForLvlUp"
            >

            </progress>

            <span class="progress-value">
               {{ user.point }}/{{ user.scoreForLvlUp }}
            </span>

          </div>

          <p>До нового уровня: {{ user.scoreForLvlUp - user.point }}</p>

          <p class="infotext">Очков на прокачку :{{ user.score }}</p>

          <p class="infotext">Уровень : {{ user.lvl }}</p>

          <button class="infoButton" @click="choice">В БОЙ</button>

        </div>

      </div>

      <div class="choiceenemies" v-if="isFour">

        <div class="divOfEnemies" v-for="enemy in enemies" v-bind:key="enemy">

          <p class="textInDivOfEnemy">{{ enemy.name }}</p>

          <p class="textInDivOfEnemy">ХП: {{ enemy.HP }}</p>

          <p class="textInDivOfEnemy">Урон: {{ enemy.damage }}</p>

          <p class="textInDivOfEnemy">
             После победы очков получено: {{ enemy.defeatPoint }}
          </p>

          <button class="buttonFight" @click="fight(enemy)"> В бой </button>

        </div>

      </div>

      <div class="fightdiv" v-if="isFive">

        <div class="fightDiv" v-for="round in rounds" v-bind:key="round">

          <p class="fightText">Раунд {{ round }}</p>

          <p class="fightText">
             Вы нанесли противнику {{ cloneUser.damage }} урона
          </p>

          <p class="fightText">
             У {{ cloneEnemy.name }} осталось {{ cloneEnemy.HP - cloneUser.damage
            * round }} ХП
          </p>

          <p class="fightText">Вам нанесли{{ cloneEnemy.damage }} урона</p>

          <p class="fightText">
             У вас осталось {{ cloneUser.HP - cloneEnemy.damage * round }} ХП
          </p>

        </div>

      </div>

    </div>

  </div>

</template>

<script>
// import { enemies } from "./game.js";
export default {
  name: 'App',
  data() {
    return {
      isFirst: true,
      isSecond: false,
      isThierd: false,
      isFour: false,
      isFive: false,
      user: {
        name: '',
        HP: 222,
        damage: 1,
        lvl: 0,
        point: 0,
        score: 0,
        scoreForLvlUp: 5,
      },
      enemies: [
        { HP: 1, damage: 12, name: 'ww', defeatPoint: 5 },
        { HP: 1, damage: 1, name: 'www', defeatPoint: 4 },
      ],
    }
  },
  methods: {
    first() {
      this.isFirst = false
      this.isSecond = true
    },
    addName() {
      if (this.user.name.length < 3) {
        console.log('korot')
      } else {
        this.isSecond = false
        this.isThierd = true
      }
    },
    choice() {
      this.isFour = true
    },
    fight(enemy) {
      this.isFour = false
      this.isFive = true
      console.log('Вы начали бой с ' + enemy)
      this.cloneEnemy = {}
      this.cloneUser = {}
      Object.assign(this.cloneEnemy, enemy)
      Object.assign(this.cloneUser, this.user)
      this.rounds = []
      const cloneEnemy = {}
      const cloneUser = {}
      Object.assign(cloneEnemy, enemy)
      Object.assign(cloneUser, this.user)
      for (let i = 1; cloneEnemy.HP > 0 && cloneUser.HP > 0; i++) {
        this.rounds.push(i)
        cloneEnemy.HP -= cloneUser.damage
        cloneUser.HP -= cloneEnemy.damage
      }
    },
  },
}
</script>
