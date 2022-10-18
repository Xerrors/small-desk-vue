<script setup>

import { onMounted, reactive } from 'vue'

const current = reactive({
  hour: 0,
  minute: 0,
  second: 0
})

// 获取当前时间
const getTime = () => {
  const date = new Date()
  current.hour = date.getHours() < 10 ? '0' + date.getHours() : date.getHours()
  current.minute = date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()
  current.second = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds()
}

// 每秒更新时间
const updateTime = () => {
  setInterval(() => {
    getTime()
  }, 1000)
}

onMounted(() => {
  getTime()
  updateTime()
})
</script>

<template>
  <div class="time-container fullscreen auto-flex colorful-bg">
      <span class="hour">{{ current.hour }}</span>
      <!-- TODO: 添加闪烁 -->
      <span class="colon auto-flex">
        <div class="colon-dot"></div>
        <div class="colon-dot"></div>
      </span>
      <span class="minute">{{ current.minute }}</span>
      <!-- <span>:</span>
      <span class="second">{{ current.second }}</span> -->
  </div>
</template>
  
<style scoped>
.time-container {
  padding: 2rem;
}
.colon {
  position: relative;
  width: 10%;
  display: flex;
  gap: 8vw;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  animation: blink 1s infinite;
}

.colon-dot {
  width: 4vw;
  height: 4vw;
  border-radius: 50%;
  background: #fff;
}

.hour, .minute {
  width: 42%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  /* background-color: var(--vt-c-black-soft); */
  border-radius: 4vw;
  font-size: 30vw;
  font-weight: 700;
  color: #fff;
}

@media screen and (max-width: 768px) {
  .hour, .minute {
    width: 100%;
    height: 42%;
    font-size: 30vh;
    border-radius: 4vh;
  }
  .colon {
    flex-direction: row;
    width: 100%;
    height: 10%;
    gap: 8vh;
  }
  
  .colon-dot {
    width: 4vh;
    height: 4vh;
    border-radius: 50%;
    background: #fff;
  }
}

@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>