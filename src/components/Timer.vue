<script setup>
import StartIcon from './icons/Start.vue'
import StopIcon from './icons/Stop.vue'
import PauseIcon from './icons/Pause.vue'

function toHoursAndMinutes(totalSeconds) {
  const totalMinutes = Math.floor(totalSeconds / 60)

  const seconds = totalSeconds % 60
  const hours = Math.floor(totalMinutes / 60)
  const minutes = totalMinutes % 60

  return `${hours}:${minutes}:${seconds}`
}
</script>

<template>
  <div class="timer">
    <div class="timer__screen" :style="{
      color: timer.isRunning ? '#ffff' : '#9E9E9E',
      borderBottom: timer.isRunning ? '1px solid #ffff' : '1px solid #9E9E9E'
    }">
      <span>{{ toHoursAndMinutes(timer.count) }}</span>
    </div>
    <div class="timer_control">
      <button type="button" :id="timerId" class="timer_btn start_btn" @click="startTimer">
        <StartIcon v-if="timer.count >= 0 && !timer.isRunning" fill="#9E9E9E" />
        <PauseIcon v-else fill="#ffff" />
      </button>
      <button type="button" :id="timerId" class="timer_btn stop_btn" @click="clearTimer">
        <StopIcon :fill="timer.isRunning ? '#ffff' : '#9E9E9E'" />
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['timer', "timerId"],

  data: function () {
    return {
      timerControl: null
    }
  },

  methods: {
    nextTick: (doStep, time = 1000) => {
      let nextAt, timeout
      nextAt = new Date().getTime() + time

      const wrapper = () => {
        nextAt += time;
        timeout = setTimeout(wrapper, nextAt - new Date().getTime())
        doStep()
      }

      timeout = setTimeout(wrapper, nextAt - new Date().getTime())

      const clear = () => clearTimeout(timeout)

      return { clear };
    },

    startTimer() {
      if (!this.timer.isRunning) {
        const timerControl = this.nextTick(() => this.timer.count++)
        this.timer.isRunning = true
        this.timerControl = timerControl
      } else {
        this.pauseTimer()
      }
    },
    pauseTimer() {
      this.timer.isRunning = false
      this.timerControl.clear()
    },
    clearTimer() {
      this.timer.count = 0
      this.timer.isRunning = false
      this.timerControl.clear()
    }
  }
}
</script>

<style>
.timer {
  margin: 10px;
  max-width: 225px;
  height: 120px;
  background-color: #696969;
  border-radius: 5px;
}

.timer__screen {
  text-align: center;
  font-weight: 400;
  font-size: 22px;
  line-height: 21px;
  padding: 22px 75px 20px 75px;
  border-bottom: 1px solid #9e9e9e;
}

.timer_control {
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 20px 50px;
}


.timer_btn {
  background: none;
  border: none;
  outline: none;
  width: 20px;
  height: 20px;
  cursor: pointer;
}
</style>
