<script setup>
import { ref } from 'vue'

// get the current time
const getTime = () => {
  const date = new Date()
  const hour = date.getHours() < 10 ? '0' + date.getHours() : date.getHours()
  const minute = date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()
  return `${hour}:${minute}`
}

// get the current date
const getDate = () => {
  const date = new Date()
  const year = date.getFullYear()
  const month = date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1
  const day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate()
  return `${year}-${month}-${day}`
}

// get the current week
const getWeek = () => {
  const date = new Date()
  const week = date.getDay()
  const weekMap = {
    0: '星期日',
    1: '星期一',
    2: '星期二',
    3: '星期三',
    4: '星期四',
    5: '星期五',
    6: '星期六'
  }
  return weekMap[week]
}

const time = ref(getTime())
const date = ref(getDate())
const week = ref(getWeek())

</script>

<template>
  <div class="datetime">
    <div class="date">
      <div>{{ date }}</div>
      <div>{{ week }}</div>
    </div>
    <div class="time">
      <span>{{ time }}</span>
    </div>
  </div>
</template>

<style lang="less" scoped>
.datetime {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  padding: 1.5rem;
  gap: 1rem;
  text-align: center;
}

.date {
  width: 100%;
  display: flex;
  justify-content: space-around;
  font-size: 1.2rem;
  line-height: 1.5rem;
}

.time {
  display: flex;
  gap: 1rem;
  font-size: 5rem;
  line-height: 5rem;
  font-weight: bold;
  letter-spacing: 2px;
}


</style>