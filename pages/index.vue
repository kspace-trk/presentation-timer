<template>
  <div class="main-wrapper">
    <div class="main-contents">
      <div class="menu-wrapper" v-if="!timerPage">
        <button @click="startPresent">発表開始</button>
        <button @click="startQnA">質疑応答開始</button>
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
  </div>
</template>

<script>
import timer from '@/components/timer.vue'
import clickSound1 from '~/assets/sound/voice1.mp3'
import clickSound2 from '~/assets/sound/voice2.mp3'
import clickSound3 from '~/assets/sound/voice3.mp3'
const voice1 = new Audio(clickSound1)
const voice2 = new Audio(clickSound2)
const voice3 = new Audio(clickSound3)
export default {
  name: 'timer',
  data() {
    return {
      min: 2,
      sec: 0,
      timerOn: false,
      timerPage: false,
      timerObj: null,
      presentState: false, //終了後に音声を鳴らすかどうか
    }
  },
  methods: {
    count() {
      if (this.sec <= 0 && this.min >= 1) {
        this.min--
        this.sec = 59
      } else if (this.sec <= 0 && this.min <= 0) {
        this.complete()
      } else {
        this.sec--
      }
    },

    start() {
      let self = this
      this.timerObj = setInterval(function () {
        self.count()
      }, 1000)
      this.timerOn = true //timerがONであることを状態として保持
    },
    changeState() {
      this.presentState = true
    },
    startPresent() {
      this.timerPage = true
      voice1.play()
      setTimeout(this.start, 4000)
      setTimeout(this.changeState, 4000)
    },
    startQnA() {
      this.timerPage = true
      voice3.play()
      setTimeout(this.start, 7500)
      this.presentState = false
    },
    stop() {
      clearInterval(this.timerObj)
      this.timerOn = false //timerがOFFであることを状態として保持
    },

    complete() {
      clearInterval(this.timerObj)
      this.timerOn = false //timerがOFFであることを状態として保持
      this.timerPage = false
      if (this.sec == 0 && this.min == 0 && this.presentState) {
        voice2.play()
      } else if (this.sec >= 0 && this.min >= 0) {
        voice2.pause()
      }
      this.min = 2
      this.sec = 0
    },
  },

  computed: {
    formatTime() {
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
.main-contents {
  max-width: 1000px;
  width: 100%;
  height: 100vh;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
}
.menu-wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
@media screen and (max-width: 480px) {
  .menu-wrapper {
    width: 100%;
    height: 50%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
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
