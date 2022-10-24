<script setup>
import { shallowRef, ref, defineAsyncComponent, reactive } from 'vue'
import DateTimeBoard from '../components/dashboard/DateTimeBoard.vue';
import CountDownMini from '../components/dashboard/CountDownMini.vue';
// import CountDownHarf from '../components/dashboard/CountDownHarf.vue';
import BatteryItem from '../components/dashboard/BatteryItem.vue';
import PomodoroBoardMiddle from '../components/dashboard/PomodoroBoardMiddle.vue';

const state = reactive({
  pomodoro: {
    started: false,
  }
})

const curCompName = ref('CountDownHarf');
const curCompProps = ref({});
const curComp = shallowRef(defineAsyncComponent(() => import(`../components/dashboard/${curCompName.value}.vue`)));

const getCurComp = () => {
  return defineAsyncComponent(() => import(`../components/dashboard/${curCompName.value}.vue`));
}

const activateComponent = (name, props=null) => {
  curCompName.value = name;
  curCompProps.value = props;
  curComp.value = getCurComp();
}

const handlePomodoro = (time) => {
  state.pomodoro.started = true;
  activateComponent('PomodoroBoradHarf', {mins: time});
}

const debug = () => {
  state.pomodoro.started = false;
  activateComponent('CountDownHarf');
}

</script>

<template>
  <div class="fullscreen auto-flex app-kit">
    <div class="first">
      <DateTimeBoard class="grid-3-2" />
      <CountDownMini @click="activateComponent('CountDownHarf')"/>
      <div @click="activateComponent('ImageGallaryHalf')">M</div>
      <div class="grid-2-1">WiFi</div>
      <BatteryItem  class="grid-2-1"/>
      <div v-show="!state.pomodoro.started" @click="handlePomodoro(10)">10</div>
      <div v-show="!state.pomodoro.started" @click="handlePomodoro(25)">25</div>
      <div v-show="!state.pomodoro.started" @click="handlePomodoro(45)">45</div>
      <div v-show="!state.pomodoro.started" @click="handlePomodoro(90)">90</div>
      <PomodoroBoardMiddle class="grid-4-1" v-if="state.pomodoro.started" @click="debug"/>
    </div>
    <div class="second">
      <div class="grid-4-4">
        <!-- <CountDownHarf /> -->
        <!-- <PomodoroBoradHarf /> -->
        <KeepAlive>
          <component :is="curComp" class="board" :data="curCompProps"></component>
        </KeepAlive>
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
  transition: all 0.1s ease-out;
}

.first>div:active {
  transform: scale(0.9);
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

.grid-4-1 {
  grid-column-start: span 4;
  grid-row-start: span 1;
}
</style>