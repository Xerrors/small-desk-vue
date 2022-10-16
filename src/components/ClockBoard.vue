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
  <div>
    <div class="time-container">
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
  </div>
</template>
  
<style scoped>
.time-container {
  width: 100vw;
  height: 100vh;
  display: flex;
  background: black;
}

.time {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 35vw;
  font-weight: 700;
  color: #fff;
}
.colon {
  position: relative;
  width: 8vw;
  height: 100%;
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

/* @media screen and (max-width: 768px) {
  .time {
    font-size: 100px;
  }

  .colon {
    width: 2rem;
    gap: 1rem;
  }

  .colon-dot {
    width: 0.8rem;
    height: 0.8rem;
  }
} */

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