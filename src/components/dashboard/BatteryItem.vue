<script setup>
import { computed } from '@vue/reactivity';
import { useBattery } from '@vueuse/core'

const { charging, chargingTime, dischargingTime, level } = useBattery()

const battery_level = computed(() => {
  return Math.floor(level.value * 100)
})
</script>

<template>
  <div>
    <div class="icon"></div>
    <div class="battery">
      <div class="battery-level" :style="{ width: battery_level + '%' }"></div>
      <div class="battery-text">{{ battery_level }}%</div>
    </div>
  </div>
</template>

<style lang="less" scoped>
.battery {
  display: flex;
  position: relative;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 0.5rem;
  width: 100%;
  height: 100%;
}

.battery-level {
  position: absolute;
  height: 100%;
  // background: rgba(0,255,0,0.15);
}

// .battery-text {
//   font-size: 1.2rem;
//   line-height: 1.5rem;
//   color: #fff;
// }


</style>