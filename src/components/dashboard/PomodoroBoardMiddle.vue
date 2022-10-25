<script setup>
import { reactive, ref } from 'vue'
import { 
  PauseCircleTwoTone,
  PlayCircleTwoTone,
  PauseOutlined,
  CaretRightFilled
} from '@ant-design/icons-vue'


const props = defineProps(['pomodoro'])
const emit = defineEmits(['on-pause', 'on-continue', 'on-stop'])

const pomodoro = ref(props.pomodoro)


const pauseAndContinue = () => {
  if (pomodoro.value.isPaused) {
    emit('on-continue')
  } else {
    emit('on-pause')
  }
}

</script>

<template>
  <div class="pomodoro-board-middle" id="pomodoro-board-middle">
    <div class="bg-bar" :style="pomodoro.timerTimeLeftPercentStyle"></div>
    <div class="pomodoro-middle-left">{{ pomodoro.timerTimeLeftStr }}</div>
    <div class="pomodoro-middle-max"></div>
    <div class="pomodoro-middle-max"></div>
    <div class="pomodoro-middle-max"></div>
    <div class="pomodoro-middle-max"></div>
    <div class="pomodoro-middle-max"></div>
    <!-- <div class="pomodoro-middle-max">{{ pomodoro.mins }}mins</div> -->
    <div class="button" @click.prevent="pauseAndContinue" v-show="!pomodoro.isFinished">
      <PauseOutlined v-show="!pomodoro.isPaused"/>
      <CaretRightFilled v-show="pomodoro.isPaused"/>
    </div>
    <div class="button shutdown" @click.prevent="$emit('on-stop')" v-show="!pomodoro.isFinished"><div></div></div>
    <div class="button done" v-show="pomodoro.isFinished" @click.prevent="$emit('on-stop')">Done</div>

  </div>
</template>

<style lang="less" scoped>
#pomodoro-board-middle.pomodoro-board-middle {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  padding: 0 1rem;
  z-index: 0;
  // gap: 1rem;
}

div.bg-bar {
  position: absolute;
  top: 0;
  left: 0;
  // width: 100%;
  height: 100%;
  background-color: #00b578;
  opacity: 0.2;
  z-index: -1;
}

div.pomodoro-board-middle > div {
  font-size: 1.5rem;
}
div.pomodoro-board-middle > div.pomodoro-middle-left {
  font-size: 2rem;
  color: #00b578;
  font-weight: 700;
}

div.pomodoro-board-middle > div.button {
  display: flex;
  justify-content: center;
  align-items: center;
  color: var(--vt-c-black-mute);
  font-size: 1.5rem;
  border: none;
  width: 3rem;
  height: 3rem;
  background-color: #00b578;
  border-radius: 50%;
  cursor: pointer;
}
div.pomodoro-board-middle > div.button.shutdown {
  background-color: #fa5151;
}
div.pomodoro-board-middle > div.button.done {
  background-color: #00b578;
  color: white;
  font-size: 1.2rem;
  border-radius: 8px;
  width: auto;
  height: auto;
  padding: 0.5rem 1rem;
}

div.pomodoro-board-middle > div.button.shutdown > div {
  width: 1rem;
  height: 1rem;
  background-color: var(--vt-c-black-mute);
  margin: 0 auto;
}

</style>