<template>
  <div class="main-wrapper">
    <div class="menu-wrapper" v-if="!timerPage">
      <button @click="changePage">発表開始</button>
      <button>質疑応答開始</button>
    </div>
    <div class="timer-wrapper" v-if="timerPage">
      <div class="timer">
        {{ formatTime }}
      </div>
      <div class="timer-btn">
        <button @click="start" v-if="!timerOn">スタート</button>
        <button @click="stop" v-if="timerOn">ストップ</button>
        <button @click="complete">リセット</button>
      </div>
    </div>
  </div>
</template>

<script>
import timer from '@/components/timer.vue'
export default {
  name: 'timer',
  data() {
    return {
      min: 2,
      sec: 0,
      timerOn: false,
      timerPage: false,
      timerObj: null,
    }
  },
  methods: {
    changePage() {
      this.timerPage = true
    },
    count: function () {
      if (this.sec <= 0 && this.min >= 1) {
        this.min--
        this.sec = 59
      } else if (this.sec <= 0 && this.min <= 0) {
        this.complete()
      } else {
        this.sec--
      }
    },

    start: function () {
      let self = this
      this.timerObj = setInterval(function () {
        self.count()
      }, 1000)
      this.timerOn = true //timerがOFFであることを状態として保持
    },

    stop: function () {
      clearInterval(this.timerObj)
      this.timerOn = false //timerがOFFであることを状態として保持
    },

    complete: function () {
      clearInterval(this.timerObj)
      this.timerOn = false //timerがOFFであることを状態として保持
      this.timerPage = false
      this.min = 2
      this.sec = 0
    },
  },
  computed: {
    formatTime: function () {
      let timeStrings = [this.min.toString(), this.sec.toString()].map(
        function (str) {
          if (str.length < 2) {
            return '0' + str
          } else {
            return str
          }
        }
      )
      return timeStrings[0] + ':' + timeStrings[1]
    },
  },
  components: {
    timer,
  },
}
</script>

<style>
.main-wrapper {
  width: 100%;
  height: 100vh;
}
.menu-wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.timer-wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.timer {
  font-size: 8rem;
  font-weight: 700;
}
.timer-btn {
  display: flex;
  justify-content: space-around;
  max-width: 600px;
  width: 90%;
}
button {
  width: 200px;
  height: 75px;
  border: 2px solid #777777;
  border-radius: 15px;
}
button:active {
  background-color: #777777;
  color: #ffffff;
}
</style>
