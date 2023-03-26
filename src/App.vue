<script setup>
import Timer from './components/Timer.vue'
import AddTimerIcon from './components/icons/Add.vue'
import CloseTimerIcon from './components/icons/Close.vue'
</script>

<template>
  <main class="main">
    <div v-for="(timer, timerId) in timers" :key="timerId">
      <button class="timer_btn close_btn" :id="timerId" @click="removeTimer">
        <CloseTimerIcon />
      </button>
      <Timer :timer="timer" :timerId="timerId" />
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

.close_btn {
  position: absolute;
}

@media (max-width: 600px) {
  .main {
    grid-template-columns: repeat(1, 1fr);
  }
}
</style>
