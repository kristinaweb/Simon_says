<template>
  <div class="container">
    <div class="square_container" :class="flag ? 'disabled': ''">
      <div @click="play(1)" class="square square--red" :class="squares.square_1 ? 'shadow': ''"></div>
      <div @click="play(2)" class="square square--blue" :class="squares.square_2 ? 'shadow': ''"></div>
      <div @click="play(3)" class="square square--yellow" :class="squares.square_3 ? 'shadow': ''"></div>
      <div @click="play(4)" class="square square--green" :class="squares.square_4 ? 'shadow': ''"></div>
    </div>
    <div>
      <div class="container_controll" :class="startButton ? 'disabled' : ''">
        <p>Сложность:</p>
        <div class="controll">
          <span>Легкая</span>
          <input type="radio" name="mode" value="easy" checked v-model="picked" />
        </div>
        <div class="controll">
          <span>Средняя</span>
          <input type="radio" name="mode" value="normal" v-model="picked" />
        </div>
        <div class="controll">
          <span>Тяжелая</span>
          <input type="radio" name="mode" value="hard" v-model="picked" />
        </div>
      </div>
      <button :class="startButton ? 'disabled' : ''" @click="start">Старт</button>
      <p>
        <span>Раунд</span>
        {{round}}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Game",
  props: {
    msg: String,
  },
  data() {
    return {
      round: 1,
      arrayRound: [],
      squares: {
        square_1: false,
        square_2: false,
        square_3: false,
        square_4: false,
      },
      myArrayRound: [],
      flag: true,
      startButton: false,
      picked: "easy",
    };
  },
  methods: {
    /*eslint-disable */
    start() {
      this.flag = true;
      this.startButton = true;
      let roundItem = this.round;
      for (let i = 0; i < roundItem; i++) {
        let item = this.random(1, 4);
        this.arrayRound.push(item);
      }
      let difficulty;
      let animate;
      switch (this.picked) {
        case "easy":
          difficulty = 1500;
          animate = 1000;
          break;
        case "normal":
          difficulty = 1000;
          animate = 500;
          break;
        case "hard":
          difficulty = 400;
          animate = 300;
          break;
        default:
          break;
      }

      this.arrayRound.forEach((e, index) => {
        let item = "square_" + String(e);
        let delay = (index + 1) * difficulty;
        setTimeout(() => {
          let audio = new Audio(require("../assets/" + e + ".mp3"));
          audio.play();
          this.squares[item] = true;
          setTimeout(() => {
            this.squares[item] = false;
            if (this.arrayRound.length - 1 == index) {
              this.flag = false;
            }
          }, animate);
        }, delay);
      });
    },
    random(min, max) {
      let rand = min + Math.random() * (max + 1 - min);
      return Math.floor(rand);
    },
    play(index) {
      let audio = new Audio(require("../assets/" + index + ".mp3"));
      audio.play();
      let myArrayRound = this.myArrayRound;
      let arrayRound = this.arrayRound;

      if (arrayRound.length) {
        myArrayRound.push(index);

        for (let i = 0; i < myArrayRound.length; i++) {
          if (myArrayRound[i] == arrayRound[i]) {
            if (myArrayRound.length == arrayRound.length) {
              this.myArrayRound = [];
              this.arrayRound = [];
              this.round = this.round + 1;
              this.start();
            }

            return true;
          } else {
            alert("Проиграли");
            this.round = 1;
            this.myArrayRound = [];
            this.arrayRound = [];
            this.flag = true;
            this.startButton = false;
          }
        }
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* html,
body {
  box-sizing: border-box;
} */
.disabled {
  pointer-events: none;
}
.container {
  display: flex;
  justify-content: center;
  margin-top: 50px;
}
.controll {
  margin-top: 20px;
  margin-bottom: 20px;
}
.square_container {
  width: 310px;
  height: 300px;
  display: flex;
  flex-wrap: wrap;
  margin-right: 50px;
}
.square {
  width: 150px;
  height: 150px;
}
.shadow {
  transition: 1s;
  box-shadow: 0px 2px 35px 0px rgba(0, 0, 0, 0.75);
}
.square--red {
  border-bottom: 2px solid transparent;
  border-top-left-radius: 100%;
  background-color: #ff5643;
}
.square--red:hover {
  border-left: 2px solid black;
  border-top: 2px solid black;
}
.square--blue {
  border-bottom: 2px solid transparent;
  border-top-right-radius: 100%;
  background-color: dodgerblue;
}
.square--blue:hover {
  border-right: 2px solid black;
  border-top: 2px solid black;
}
.square--yellow {
  border-bottom: 2px solid transparent;
  border-bottom-left-radius: 100%;
  background-color: #feef33;
}
.square--yellow:hover {
  border-left: 2px solid black;
  border-bottom: 2px solid black;
}
.square--green {
  border-bottom: 2px solid transparent;
  border-bottom-right-radius: 100%;
  background-color: #bede15;
}
.square--green:hover {
  border-right: 2px solid black;
  border-bottom: 2px solid black;
}
</style>
