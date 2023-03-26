<script setup>
import Timer from './components/Timer.vue'
import AddTimerIcon from './components/icons/Add.vue'
import CloseTimerIcon from './components/icons/Close.vue'
import * as uuid from "uuid"
</script>

<template>
  <main class="main">
    <div v-for="(timer) in timers" :key="timer.id">
      <button class="timer_btn close_btn" :id="timer.id" @click="removeTimer">
        <CloseTimerIcon />
      </button>
      <Timer :timer="timer" />
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
        id: uuid.v4(),
        count: 0,
        isRunning: false
      })
    },
    removeTimer(ev) {
      const index = this.timers.findIndex((item) => item.id === ev.currentTarget.id)
      this.timers.splice(index, 1)
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
