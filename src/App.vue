<template>
  <div
    :class="{
      'yellow-background': isYellowBackground,
      'blue-background': isBlueBackground,
      'red-background': isRedBackground,
    }"
    @click="handleScreenClick"
    class="app"
  >
    <div v-if="isT1Running">
      <h1 style="font-size: 12em">{{ formatTime(t1) }}</h1>
    </div>
    <div v-if="isT2Running">
      <h1 style="font-size: 6em">{{ formatTime(t2) }}</h1>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      defaultTime: 30,
      isT1Running: false,
      isT2Running: false,
      t1Interval: null,
      t2Interval: null,
      t1: 30,
      t2: 30,
      isBlueBackground: true,
      isYellowBackground: false,
      isRedBackground: false,
    };
  },
  watch: {
    t1(newValue) {
      if (newValue <= 0) {
        if (this.isT2Running) {
          this.t1 = this.t2;
          clearInterval(this.t2Interval);
          this.isT2Running = false;
          this.t2 = 0;
          this.isBlueBackground = false;
          this.isYellowBackground = true;
          this.isRedBackground = false;
        } else {
          clearInterval(this.t1Interval);
          this.t1 = 5;
          this.isT1Running = false;
          this.isBlueBackground = true;
          this.isYellowBackground = false;
          this.isRedBackground = false;
        }
      }
    },
    // t2(newValue) {},
  },
  computed: {
    formatTime() {
      return function (time) {
        const seconds = Math.floor(time % 60);
        const decimal = Math.floor((time % 1) * 10); // 小数部分を取得
        return `${seconds.toString().padStart(1, "0")}.${decimal}`; // 小数第一位まで表示
      };
    },
  },
  methods: {
    startTimerT1() {
      this.isT1Running = true;
      this.isBlueBackground = false;
      this.isYellowBackground = true;
      this.isRedBackground = false;
      this.t1 = this.defaultTime;

      this.t1Interval = setInterval(() => {
        this.t1 -= 0.01;
      }, 10);
    },
    startTimerT2() {
      this.isT1Running = true;
      this.isBlueBackground = false;
      this.isYellowBackground = false;
      this.isRedBackground = true;
      this.t2 = this.defaultTime;

      this.t2Interval = setInterval(() => {
        this.t2 -= 0.01;
      }, 10);
    },
    handleScreenClick() {
      if (!this.isT1Running) {
        this.startTimerT1();
        this.isT1Running = true;
        return;
      }
      if (!this.isT2Running) {
        this.startTimerT2();
        this.isT2Running = true;
        return;
      }
    },
  },
};
</script>

<style scoped>
body,
html {
  height: 100%;
  margin: 0;
  padding: 0;
}
.app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw; /* 画面全体に広がるように設定 */
  transition: background-color 1s ease;
}
.yellow-background {
  background-color: yellow;
}
.red-background {
  background-color: red;
}
.blue-background {
  background-color: blue;
}
h1 {
  font-size: 4em;
  margin: 0;
}
</style>
