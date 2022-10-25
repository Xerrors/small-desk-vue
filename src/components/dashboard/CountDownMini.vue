<script setup>
import { ref } from 'vue'

const props = defineProps(['title', 'type'])
console.log('props', props)
const title = ref(props.title || '❤️')
const type = ref(props.type || 'mini')
console.log(type)
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
  <div class="count-down-mini" id="count-down-mini">
    <span class="text" v-if="type!='mini'">{{ title }}</span>
    <span class="number">{{ pastedDays }}</span>
  </div>
</template>

<style scoped>
#count-down-mini.count-down-mini {
  display: flex;
  width: 100%;
  justify-items: flex-end;
  vertical-align: middle;
  justify-content: space-around;
  align-content: space-around;
  align-items: center;
}

.count-down-mini .number {
  font-weight: bold;
}

.count-down-mini .text {
  /* font-size: 1.2rem; */
  font-weight: normal;
  line-height: 100%;
}
</style>