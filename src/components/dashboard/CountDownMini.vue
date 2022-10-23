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
  <div class="count-down-mini">
    <span class="number">{{ pastedDays }}</span>
    <span class="text"></span>
  </div>
</template>

<style scoped>
.count-down-mini {
  display: flex;
  justify-items: flex-end;
  vertical-align: middle;
}

.count-down-mini .number {
  color: rgb(255, 0, 43);
  font-weight: bold;
}

.count-down-mini .text {
  font-size: 0.8rem;
  font-weight: normal;
  line-height: 100%;
}
</style>