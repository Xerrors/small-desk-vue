<script setup>
import { ref } from 'vue'

const props = defineProps(['data'])
const pomodoro = ref(props.data)

// 设定一个时长为 25mins 的番茄钟

</script>


<template>
  <!-- 番茄钟 -->
  <div class="pomodoro" :class="{done: pomodoro.isFinished}">
    <!-- <div class="pomodoro-title">Title</div> -->
    <div class="pomodoro-time">
      <div v-if="!pomodoro.isFinished">{{ pomodoro.timerTimeLeftStr }}</div>
      <div v-else>Done</div>
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

.pomodoro.done .pomodoro-time {
  color: #00b578;
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
  border: 6px solid currentColor;
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
  border: 2px solid currentColor;
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
  border: 2px solid currentColor;
  opacity: 0.25;
}
</style>