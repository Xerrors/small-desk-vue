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
  <div class="time-container fullscreen">
    <div class="time">
      <span class="hour">{{ current.hour }}</span>
      <!-- TODO: 添加闪烁 -->
      <span class="colon">
        <div class="colon-dot"></div>
        <div class="colon-dot"></div>
      </span>
      <span class="minute">{{ current.minute }}</span>
      <!-- <span>:</span>
      <span class="second">{{ current.second }}</span> -->
    </div>
  </div>
</template>
  
<style scoped>
.time {
  display: flex;
  justify-content: center;
  align-items: center;
}
.colon {
  position: relative;
  width: 10vw;
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
  width: 40vw;
  height: 40vw;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #181a1c;
  border-radius: 4vw;
  font-size: 30vw;
  font-weight: 700;
  color: #fff;
}

@media screen and (max-width: 768px) {
  .time {
    flex-direction: column;
  }
  .hour, .minute {
    width: 40vh;
    height: 40vh;
    font-size: 30vh;
    border-radius: 4vh;
  }
  .colon {
    flex-direction: row;
    width: 100%;
    height: 10vh;
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