<template>
  <div>
    <div class="simon">
      <div class="area">
        <div
          v-for="area of areas"
          :class="[area.color, { opacity: !area.checked }]"
          :key="area.id"
          @click="chooseColor(area.id)"
        ></div>
      </div>
      <div class="info">
        <div class="count">
          <div v-if="count === 0 && !gameOver">
            <h3>Нажмите "Старт" для начала</h3>
          </div>
          <div v-else-if="count > 0 && !gameOver">
            <h3>Раунд: {{ count }}</h3>
          </div>
          <div class="gameover" v-else-if="gameOver">
            <h3>Вы проиграли!</h3>
          </div>
          <button class="start" @click="startGame">Старт</button>
        </div>
        <div class="options">
          <span>
            <input type="radio" id="easy" value="1.5" v-model="level" />
            <label for="easy">Легкий</label>
          </span>
          <span>
            <input type="radio" id="middle" value="1" v-model="level" />
            <label for="middle">Средний</label>
          </span>
          <span>
            <input type="radio" id="hard" value="0.4" v-model="level" />
            <label for="hard">Сложный</label>
          </span>
        </div>
        <div class="round">Ваш рекорд: {{ record }} раунд</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "simon",
  data: () => ({
    areas: [
      {
        id: 1,
        color: "blue",
        checked: false,
        audio: new Audio(
          "https://zvukipro.com/uploads/files/2019-09/1567587165_3a7ee48dc00822f.mp3"
        ),
      },
      {
        id: 2,
        color: "red",
        checked: false,
        audio: new Audio(
          "https://zvukipro.com/uploads/files/2019-09/1567587229_8af5b2bf5d19c00.mp3"
        ),
      },
      {
        id: 3,
        color: "yellow",
        checked: false,
        audio: new Audio(
          "https://zvukipro.com/uploads/files/2019-09/1567587462_453ec44415174e0.mp3"
        ),
      },
      {
        id: 4,
        color: "green",
        checked: false,
        audio: new Audio(
          "https://zvukipro.com/uploads/files/2019-09/1567587261_fcec4eee249d72c.mp3"
        ),
      },
    ],
    level: "1.5",
    arrayLevel: [],
    copyArray: [],
    count: 0,
    active: false,
    gameOver: false,
    record: 0,
  }),
  methods: {
    chooseColor(id) {
      if (this.active) {
        const check = this.areas.find((a) => a.id === id);
        check.audio.play();
        this.turnOn(check);
        const desired = this.copyArray.shift();
        const acti = desired === id;
        if (this.copyArray.length === 0 && acti) {
          this.newRound();
        } else if (!acti) {
          this.endGame();
        }
      }
    },

    turnOn(check) {
      check.checked = !check.checked;
      setTimeout(() => {
        check.checked = !check.checked;
      }, 300);
    },

    startGame() {
      if (!this.active) {
        this.gameOver = false;
        this.newRound();
      }
    },

    endGame() {
      if (this.count > this.record) {
        this.record = this.count;
      }
      this.count = 0;
      this.arrayLevel = [];
      this.active = false;
      this.gameOver = true;
    },

    randomNumber() {
      return Math.floor(Math.random() * 4 + 1);
    },

    newRound() {
      this.count++;
      this.arrayLevel.push(this.randomNumber());
      this.copyArray = this.arrayLevel.concat();
      this.taskForPlayer();
    },

    taskForPlayer() {
      this.active = false;
      let score = 0;
      const interval = setInterval(() => {
        const check = this.areas.find((a) => a.id === this.arrayLevel[score]);
        check.audio.play();
        this.turnOn(check);
        score++;
        if (score >= this.arrayLevel.length) {
          clearInterval(interval);
          this.active = true;
        }
      }, Number(this.level) * 1000);
    },
  },
};
</script>

<style scoped>
.simon {
  display: flex;
  justify-content: space-between;
  box-shadow: 2px 3px 5px rgba(0, 0, 0, 0.5);
  border-radius: 5px;
  padding: 10px;
}
.area {
  width: 45%;
  display: flex;
  flex-wrap: wrap;
}
.blue,
.red,
.yellow,
.green {
  width: 150px;
  height: 150px;
  margin: 3px;
  box-shadow: 2px 3px 5px rgba(0, 0, 0, 0.5);
  border-radius: 50%;
  cursor: pointer;
}
.blue {
  background: blue;
}
.red {
  background: red;
}
.yellow {
  background: yellow;
}
.green {
  background: green;
}
.options {
  display: flex;
  flex-direction: column;
}
.info {
  width: 45%;
}
.count {
  margin-bottom: 30px;
}
input {
  margin-bottom: 10px;
}
.opacity {
  opacity: 0.4;
}
.start {
  padding: 5px;
  background: rgb(75, 255, 75);
  border-radius: 5px;
  outline: none;
  font-size: 1rem;
  cursor: pointer;
}
.gameover {
  background: rgb(255, 173, 173);
  text-align: center;
  border-radius: 5px;
}
.round {
  margin-top: 30px;
}
</style>
