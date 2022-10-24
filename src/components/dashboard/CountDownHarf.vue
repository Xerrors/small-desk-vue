<script setup>
import { ref } from 'vue'
// 计算距离日期已经过去的天数
const getPastedDays = () => {
  const targetDate = new Date()
  const startDate = new Date("2022-09-25")
  const diff = targetDate.getTime() - startDate.getTime()
  return Math.floor(diff / (1000 * 60 * 60 * 24))
}

const pastedDays = ref(getPastedDays())

// 每隔 1 分钟更新时间和日期
setInterval(() => {
  pastedDays.value = getPastedDays()
}, 1000 * 60)


</script>

<template>
  <div class="pastdays auto-flex">
    <div>在一起的天数❤️</div>
    <div class="days">{{ pastedDays }}</div>
  </div>
</template>

<style scoped>
.pastdays {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  font-size: 2rem;
  color: #e4e4e4;
}

.days {
  font-size: 8rem;
  line-height: 10rem;
  font-weight: 700;
  color: white;
}
</style>