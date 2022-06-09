<template>
  <div>
    <div v-if="gameVisible" class="modal">
      <div class="modal__content">
        <button @click="showGame">Начать</button>
      </div>
    </div>
    <div class="status" v-if="!gameVisible">
      <h1>{{ status }}</h1>
    </div>
    <div class="containButton">
      <button
        class="btn"
        key="1"
        :disabled="disabledBtn"
        v-on:click="btnMethod"
        v-bind:class="{ btn_clicked: btnClass }"
      ></button>
      <button
        class="btn1"
        key="2"
        :disabled="disabledBtn"
        v-on:click="btn1Method"
        v-bind:class="{ btn1_clicked: btnClass1 }"
      ></button>
      <button
        class="btn2"
        key="3"
        :disabled="disabledBtn"
        v-on:click="btn2Method"
        v-bind:class="{ btn2_clicked: btnClass2 }"
      ></button>
      <button
        class="btn3"
        key="4"
        :disabled="disabledBtn"
        v-on:click="btn3Method"
        v-bind:class="{ btn3_clicked: btnClass3 }"
      ></button>
    </div>
    <div class="res" v-if="!gameVisible">
      <div class="score">Очки: {{ score }}</div>
      <div class="youScore">Ваши максимальные очки : {{ youScore }}</div>
    </div>
  </div>
</template>

<script>
export default {
  el: "#app",
  data() {
    return {
      gameVisible: true,
      status: "Смотри",
      score: 0,
      youScore: 0,
      arrMaxScore: [],
      btnClass: false,
      btnClass1: false,
      btnClass2: false,
      btnClass3: false,
      activeBtn: null,
      activeBtnStyle: null,
      arrRandomActiveBtn: [],
      disabledBtn: true,
      btnActiveClass: "",
      param: 1,
      btn: [],
    };
  },
  methods: {
    showGame() {
      this.gameVisible = false;
      setTimeout(() => {
        this.status = "Запоминай";
        this.randomBtnActive();
      }, 2000);
    },
    btnMethod(e) {
      this.btnClass = !this.btnClass;
      this.btnActiveClass = `${e.target.className}_clicked`;
      this.checkClickBtn(this.btnActiveClass);
      setTimeout(() => {
        this.btnClass = false;
      }, 100);
    },
    btn1Method(e) {
      this.btnClass1 = !this.btnClass1;
      this.btnActiveClass = `${e.target.className}_clicked`;
      this.checkClickBtn(this.btnActiveClass);
      setTimeout(() => {
        this.btnClass1 = false;
      }, 100);
    },
    btn2Method(e) {
      this.btnClass2 = !this.btnClass2;
      this.btnActiveClass = `${e.target.className}_clicked`;
      this.checkClickBtn(this.btnActiveClass);
      setTimeout(() => {
        this.btnClass2 = false;
      }, 100);
    },
    btn3Method(e) {
      this.btnClass3 = !this.btnClass3;
      this.btnActiveClass = `${e.target.className}_clicked`;
      this.checkClickBtn(this.btnActiveClass);
      setTimeout(() => {
        this.btnClass3 = false;
      }, 100);
    },
    randomBtnActive(param = 1) {
      let i;
      for (i = 0; i < param; i++) {
        setTimeout(() => {
          let randomNumberClass = Math.floor(Math.random() * 4);
          randomNumberClass == 0 ? (randomNumberClass = "") : "";
          this.activeBtn = `btn${randomNumberClass}`;
          this.activeBtnStyle = `btn${randomNumberClass}_clicked`;
          this.arrRandomActiveBtn.push(this.activeBtnStyle);
          let last = this.arrRandomActiveBtn.slice(-1);
          let randomBtn = this.$el.querySelector(`.${this.activeBtn}`);
          randomBtn.classList.add(`${last}`);
          setTimeout(() => {
            randomBtn.classList.remove(`${last}`);
          }, 3500);
          setTimeout(() => {
            this.status = `Ты на раунде ${i},дождись ${
              (1000 * i * 2) / 1000
            } секунд и затем кликай`;
            this.disabledBtn = false;
          }, 3700);
        }, 1000 * i * 2);
      }
    },
    checkClickBtn(el) {
      let i;
      this.disabledBtn = true;
      this.btn.push(el);
      for (i = 0; i < this.arrRandomActiveBtn.length; i++) {
        console.log(this.arrRandomActiveBtn[i] + "-Рандомное");
        console.log(this.btn[i] + "-кликнутое");
      }
      if (el == this.activeBtnStyle) {
        this.score += 1;
        this.arrMaxScore.push(this.score);
        this.youScore = Math.max.apply(null, this.arrMaxScore);
        this.status = "Успех";
        setTimeout(() => {
          this.status = "Запоминай";
          this.randomBtnActive((this.param += 1));
        }, 1000);
      } else {
        this.status = "Не правильно";
        setTimeout(() => {
          this.errorClickBtn();
        }, 500);
      }
    },
    errorClickBtn() {
      this.status = "Смотри";
      this.arrRandomBtn = [];
      this.btn = [];
      this.disabledBtn = true;
      this.score = 0;
      this.gameVisible = true;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.containButton {
  display: flex;
  justify-content: space-around;
  align-items: center;
  height: 700px;
}
.containButton button {
  border: 0;
  width: 350px;
  height: 350px;
}
.btn {
  background: rgba(0, 230, 64);
}
.btn_clicked {
  background: green;
}
.btn1 {
  background: brown;
}
.btn1_clicked {
  background: red;
}
.btn2 {
  background: darkblue;
}
.btn2_clicked {
  background: blue;
}
.btn3 {
  background: orange;
}
.btn3_clicked {
  background: orangered;
}
.modal {
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  display: flex;
}
.modal__content {
  margin: auto;
  padding: 20px;
  background: black;
  border: 2px solid white;
  border-radius: 12px;
  height: 100px;
  width: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal__content button {
  color: white;
  background: 0;
  border: 0;
  font-size: 30px;
  text-transform: uppercase;
  height: 50px;
}
.modal__content button:hover {
  cursor: pointer;
}
.res {
  display: flex;
  justify-content: space-around;
}
.status {
  text-align: center;
  padding-top: 20px;
  color: aqua;
}
</style>
