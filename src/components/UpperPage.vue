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
  <div class="fullscreen pastdays">
    <div>已经谈恋爱 <span class="days">{{ days }}</span> 天啦</div>
  </div>
</template>

<style scoped>
.pastdays {
  font-size: 8vw;
  background-color: antiquewhite;
  color: black;
}

.days {
  font-size: 20vw;
  font-weight: 700;
  color: #d13f33;
}
</style>