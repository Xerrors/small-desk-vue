<script setup>
import { reactive, computed, onMounted } from 'vue'

const props = defineProps(['data'])
const mins = props.data.mins

// 设定一个时长为 25mins 的番茄钟
const pomodoro = reactive({
  time: mins * 60,
  isRunning: false,
  isPaused: false,
  isFinished: false,
  timerId: null,
  timerTime: 1000,
  timerTimeLeft: mins * 60,
  timerTimeLeftStr: computed(() => {
    const min = Math.floor(pomodoro.timerTimeLeft / 60)
    const sec = pomodoro.timerTimeLeft % 60
    return `${min < 10 ? "0" + min : min}:${sec < 10 ? "0" + sec : sec}`
  }),
  timerTimeStr: computed(() => {
    const min = Math.floor(pomodoro.time / 60)
    const sec = pomodoro.time % 60
    return `${min < 10 ? "0" + min : min}:${sec < 10 ? "0" + sec : sec}`
  }),
  timerTimeLeftPercent: computed(() => {
    return (pomodoro.timerTimeLeft / pomodoro.time) * 100
  }),
  timerTimeLeftPercentStr: computed(() => {
    return `${pomodoro.timerTimeLeftPercent}%`
  }),
  timerTimeLeftPercentStyle: computed(() => {
    return {
      width: pomodoro.timerTimeLeftPercentStr
    }
  })
})

// 开始计时
const startTimer = () => {
  if (pomodoro.isRunning) {
    return
  }
  pomodoro.isRunning = true
  pomodoro.timerId = setInterval(() => {
    if (pomodoro.timerTimeLeft <= 0) {
      clearInterval(pomodoro.timerId)
      pomodoro.isRunning = false
      pomodoro.isFinished = true
      return
    }
    pomodoro.timerTimeLeft -= 1
  }, pomodoro.timerTime)
}

// 暂停计时
const pauseTimer = () => {
  if (pomodoro.isPaused) {
    return
  }
  pomodoro.isPaused = true
  clearInterval(pomodoro.timerId)
}

// 继续计时
const continueTimer = () => {
  if (!pomodoro.isPaused) {
    return
  }
  pomodoro.isPaused = false
  pomodoro.timerId = setInterval(() => {
    if (pomodoro.timerTimeLeft <= 0) {
      clearInterval(pomodoro.timerId)
      pomodoro.isRunning = false
      pomodoro.isFinished = true
      return
    }
    pomodoro.timerTimeLeft -= 1
  }, pomodoro.timerTime)
}

// 重置计时
const resetTimer = () => {
  clearInterval(pomodoro.timerId)
  pomodoro.isRunning = false
  pomodoro.isPaused = false
  pomodoro.isFinished = false
  pomodoro.timerTimeLeft = pomodoro.time
}

onMounted(() => {
  startTimer()
})
</script>


<template>
  <!-- 番茄钟 -->
  <div class="pomodoro">
    <!-- <div class="pomodoro-title">Title</div> -->
    <div class="pomodoro-time">
      <div>{{ pomodoro.timerTimeLeftStr }}</div>
    </div> 
  </div>
</template>

<style lang="less" scoped>
.pomodoro {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.pomodoro-time {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 14rem;
  height: 14rem;
  font-size: 3rem;
  font-weight: 700;
  color: white;
  border-radius: 50%;
  border: 6px solid #f2f4f6;
  // outline: 6px solid #232425 ;// 移动端不支持outline
  box-sizing: content-box;
}

.pomodoro-time::after {
  content: "";
  display: block;
  width: 17rem;
  height: 17rem;
  position: absolute;
  top: -1.5rem;
  left: -1.5rem;
  border-radius: 50%; 
  border: 2px solid white;
  // outline: 6px solid #232425 ;  // 移动端不支持outline
  opacity: 0.5;
}

.pomodoro-time::before {
  content: "";
  display: block;
  position: absolute;
  width: 12rem;
  height: 12rem;
  top: 1rem;
  left: 1rem;
  border-radius: 50%; 
  border: 2px solid white;
  opacity: 0.25;
}
</style>