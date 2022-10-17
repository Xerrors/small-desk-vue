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
  <div class="fullscreen pastdays auto-flex">
    <div>已经</div><div class="days">{{ days }}</div><div>天啦</div>
  </div>
</template>

<style scoped>
.pastdays {
  gap: 2rem;
  font-size: 5rem;
  color: aliceblue;
}

.pastdays {
    background-color: #CC0033;
    background-image: radial-gradient(closest-side, rgb(232, 49, 13), rgba(235, 105, 78, 0)), radial-gradient(closest-side, rgb(7, 92, 75), rgba(243, 11, 164, 0)), radial-gradient(closest-side, rgb(64, 145, 14), rgba(254, 234, 131, 0)), radial-gradient(closest-side, rgb(234, 135, 7), rgba(170, 142, 245, 0));
    background-size: 130vw 130vh, 120vw 120vh, 100vw 150vh, 120vw 130vh;
    background-position: -80vw -80vh, 30vw -20vh, -20vw 20vh, 30vw 20vh;
    background-repeat: no-repeat;
    animation: 15s moiveAnimation infinite;
}

@keyframes moiveAnimation {
    0%,
    100% {
        background-size: 130vw 130vh, 120vw 120vh, 100vw 150vh, 120vw 130vh;
        background-position: -80vw -80vh, 30vw -20vh, -20vw 20vh, 30vw 20vh;
    }
    25% {
        background-size: 120vw 120vh, 130vw 150vh, 130vw 120vh, 100vw 110vh;
        background-position: -30vw -30vh, 40vw -10vh, 0vw 10vh, -10vw 20vh;
    }
    50% {
        background-size: 130vw 130vh, 140vw 100vh, 100vw 150vh, 90vw 110vh;
        background-position: 10vw -60vh, 20vw 10vh, 10vw 30vh, 10vw -20vh;
    }
    75% {
        background-size: 140vw 140vh, 100vw 130vh, 100vw 150vh, 130vw 110vh;
        background-position: -70vw -70vh, 0vw -10vh, 30vw -20vh, 20vw 30vh;
    }
}

.days {
  font-size: 10rem;
  font-weight: 700;
  color: white;
}
</style>