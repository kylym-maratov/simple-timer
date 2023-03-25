<script setup>
import Timer from './components/Timer.vue'
import AddTimerIcon from './components/icons/Add.vue'
</script>

<template>
  <main class="main">
    <div v-for="(timer, timerId) in timers" :key="timerId">
      <Timer
        :timer="timer"
        :timerId="timerId"
        :clearTimer="clearTimer"
        :startTimer="startTimer"
        :removeTimer="removeTimer"
      />
    </div>
    <div class="add__timer" @click="addTimer">
      <AddTimerIcon />
    </div>
  </main>
</template>

<script>
export default {
  data: () => ({
    timers: []
  }),
  methods: {
    addTimer() {
      this.timers.push({
        count: 0,
        isRunning: false
      })
    },
    removeTimer(ev) {
      const timerId = this.timers.findIndex((item, i) => i === Number(ev.currentTarget.id))
      this.timers.splice(timerId, 1)
    },
    startTimer(ev) {
      this.timers = this.timers.map((item, i) =>
        i === Number(ev.currentTarget.id) ? { ...item, isRunning: !item.isRunning } : item
      )
    },
    clearTimer(ev) {
      this.timers = this.timers.map((item, i) =>
        i === Number(ev.currentTarget.id) ? { isRunning: false, count: 0 } : item
      )
    }
  }
}
</script>

<style>
.main {
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(6, 1fr);
  max-width: 100%;
}

.add__timer {
  margin: 10px;
  width: 225px;
  height: 120px;
  background-color: #696969;
  border-radius: 5px;
  cursor: pointer;
}

@media (max-width: 600px) {
  .main {
    grid-template-columns: repeat(1, 1fr);
  }
}
</style>
