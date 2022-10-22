<script setup>
import { ref } from 'vue'
const days = ref(0)

// 计算距离日期已经过去的天数
const calcDays = () => {
  const targetDate = new Date()
  const startDate = new Date("2022-09-25")
  const diff = targetDate.getTime() - startDate.getTime()
  days.value = Math.floor(diff / (1000 * 60 * 60 * 24))
}

calcDays()

</script>

<template>
  <div class="fullscreen pastdays auto-flex colorful-bg">
    <div>已经</div>
    <div class="days">{{ days }}</div>
    <div>天啦</div>
  </div>
</template>

<style scoped>
.pastdays {
  gap: 2rem;
  font-size: 5rem;
  color: aliceblue;
}

.days {
  font-size: 10rem;
  font-weight: 700;
  color: white;
}
</style>