<template>
  <div>
    <div class="timer">
      {{ formatTime }}
    </div>
    <div class="timer-btn">
      <button @click="start" v-if="!timerOn">スタート</button>
      <button @click="stop" v-if="timerOn">ストップ</button>
      <button @click="complete">リセット</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'timer',
  data() {
    return {
      min: 2,
      sec: 0,
      timerOn: false,
      timerObj: null,
    }
  },
  methods: {
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
}
</script>

<style scoped>
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
