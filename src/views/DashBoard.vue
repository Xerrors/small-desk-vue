<script setup>
import { shallowRef, ref, defineAsyncComponent, reactive, computed } from 'vue'
import DateTimeBoard from '../components/dashboard/DateTimeBoard.vue';
import CountDownMini from '../components/dashboard/CountDownMini.vue';
// import CountDownHarf from '../components/dashboard/CountDownHarf.vue';
import BatteryItem from '../components/dashboard/BatteryItem.vue';
import PomodoroBoardMiddle from '../components/dashboard/PomodoroBoardMiddle.vue';


const pomodoro = reactive({
  mins: 0,
  time: computed(() => pomodoro.mins * 60),
  isRunning: false,
  isPaused: false,
  isFinished: false,
  isClosed: true,
  timerId: null,
  timerTime: 1000,
  timerTimePassed: 0,
  timerTimeLeft: computed(() => pomodoro.time - pomodoro.timerTimePassed),
  timerTimeLeftStr: computed(() => {
    const min = Math.floor(pomodoro.timerTimeLeft / 60)
    const sec = pomodoro.timerTimeLeft % 60
    return `${min < 10 ? "0" + min : min}:${sec < 10 ? "0" + sec : sec}`
  }),
  timerTimeStr: computed(() => {
    const min = Math.floor(pomodoro.time / 60)
    const sec = pomodoro.time % 60
    return `${min < 10 ? "0" + min : min}:${sec < 10 ? "0" + sec : sec}`
  }),
  timerTimeLeftPercent: computed(() => (pomodoro.timerTimeLeft / pomodoro.time) * 100),
  timerTimeLeftPercentStr: computed(() => `${pomodoro.timerTimeLeftPercent}%`),
  timerTimeLeftPercentStyle: computed(() => {
    return {
      width: pomodoro.timerTimeLeftPercentStr
    }
  })
})

const updateTimeId = () => {
  pomodoro.timerId = setInterval(() => {
    if (pomodoro.timerTimeLeft <= 0) {
      clearInterval(pomodoro.timerId)
      pomodoro.isRunning = false
      pomodoro.isFinished = true
      return
    }
    pomodoro.timerTimePassed += 1
  }, pomodoro.timerTime)
}

// 开始计时
const startTimer = () => {
  if (pomodoro.isRunning) {
    return
  }
  pomodoro.isRunning = true
  pomodoro.isClosed = false
  updateTimeId()
}

// 暂停计时
const pauseTimer = () => {
  if (pomodoro.isPaused) {
    return
  }
  pomodoro.isPaused = true
  clearInterval(pomodoro.timerId)
}

// 继续计时
const continueTimer = () => {
  if (!pomodoro.isPaused) {
    return
  }
  pomodoro.isPaused = false
  updateTimeId()
}

const stopTimes = () => {
  clearInterval(pomodoro.timerId)
  pomodoro.isRunning = false
  pomodoro.isPaused = false
  pomodoro.isFinished = false
  pomodoro.isClosed = true
  pomodoro.timerTimePassed = 0
  if (loadedComp.value.has('PomodoroBoardHarf')) {
    loadedComp.value.delete('PomodoroBoardHarf')
  }

  console.log('curCompName.value', curCompName.value)
  if (curCompName.value === 'PomodoroBoardHarf') {
    console.log(lastActivatedComp.value)
    activateComponent(lastActivatedComp.value)
  }
}

const curCompName = ref('CountDownHarf');
const curCompProps = ref({});
const loadedComp = shallowRef(new Map());
const lastActivatedComp = ref('CountDownHarf');
const curComp = shallowRef(defineAsyncComponent(() => import(`../components/dashboard/${curCompName.value}.vue`)));

const getCurComp = () => {
  if (loadedComp.value.has(curCompName.value)) {
    curComp.value = loadedComp.value.get(curCompName.value);
  } else {
    curComp.value = defineAsyncComponent(() => import(`../components/dashboard/${curCompName.value}.vue`));
    loadedComp.value.set(curCompName.value, curComp.value);
  }
  return curComp.value;
}

const activateComponent = (name, props=null) => {
  if (name === curCompName.value) {
    return;
  }
  console.log('activateComponent', name, props)
  lastActivatedComp.value = curCompName.value;
  curCompName.value = name;
  curCompProps.value = props;
  getCurComp();
}

const handlePomodoro = (time=null) => {
  if (time) {
    pomodoro.mins = time;
    startTimer();
    activateComponent('PomodoroBoardHarf', pomodoro);
  }
}

const debug = () => {
  if (loadedComp.value.has('PomodoroBoardHarf')) {
    activateComponent('PomodoroBoardHarf');
  }
}

</script>

<template>
  <div class="fullscreen auto-flex app-kit">
    <div class="first">
      <DateTimeBoard class="grid-3-2" />
      <CountDownMini @click="activateComponent('CountDownHarf')"/>
      <div @click="activateComponent('ImageGallaryHalf')">图</div>
      <CountDownMini class="grid-2-1" type="middle"  @click="activateComponent('CountDownHarf')"/>
      <BatteryItem  class="grid-2-1"/>
      <div v-show="pomodoro.isClosed" @click="handlePomodoro(10)">10</div>
      <div v-show="pomodoro.isClosed" @click="handlePomodoro(30)">25</div>
      <div v-show="pomodoro.isClosed" @click="handlePomodoro(45)">45</div>
      <div v-show="pomodoro.isClosed" @click="handlePomodoro(90)">90</div>
      <PomodoroBoardMiddle class="grid-4-1" 
                        v-if="!pomodoro.isClosed"
                        :pomodoro="pomodoro"
                        @click.prevent="debug"
                        @on-pause="pauseTimer"
                        @on-continue="continueTimer"
                        @on-stop="stopTimes"
                        />
    </div>
    <div class="second">
      <div class="grid-4-4">
        <!-- <CountDownHarf /> -->
        <!-- <PomodoroBoardHarf /> -->
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
  overflow: hidden;
}

.first>div,
.second>div {
  background-color: var(--vt-c-black-mute);
  border-radius: 1rem;
  color: var(--vt-c-white);
  font-size: 1.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.1s ease-out;
  overflow: hidden;
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