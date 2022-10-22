<script setup>
import DateTimeBoard from '../components/dashboard/DateTimeBoard.vue';
import CountDownMini from '../components/dashboard/CountDownMini.vue';
import CountDownMiddle from '../components/dashboard/CountDownMiddle.vue';
import BatteryItem from '../components/dashboard/BatteryItem.vue';


</script>

<template>
  <div class="fullscreen auto-flex app-kit">
    <div class="first">
      <DateTimeBoard class="grid-3-2" />
      <CountDownMini />
      <div>Q</div>
      <div class="grid-2-1">WiFi</div>
      <BatteryItem  class="grid-2-1"/>
      <div>10</div>
      <div>25</div>
      <div>45</div>
      <div>90</div>
    </div>
    <div class="second">
      <div class="grid-4-4">
        <CountDownMiddle />
      </div>
    </div>
  </div>
</template>

<style scoped>
.app-kit {
  padding: 2rem;
  gap: 1.5rem;
}

.first,
.second {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr;
  gap: 1rem;
  width: 100%;
  height: 100%;
}

.first>div,
.second>div {
  background-color: var(--vt-c-black);
  border-radius: 1rem;
  color: var(--vt-c-white);
  font-size: 1.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

.grid-3-2 {
  grid-column-start: span 3;
  grid-row-start: span 2;
}

.grid-2-1 {
  grid-column-start: span 2;
  grid-row-start: span 1;
}

.grid-4-4 {
  grid-column-start: span 4;
  grid-row-start: span 4;
}
</style>