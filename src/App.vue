<script setup>
import { computed, defineAsyncComponent, reactive, ref, shallowRef } from 'vue'
import { useSwipe } from '@vueuse/core'

// 需要加载的组件集合
const components = shallowRef(new Map())
const component_list = ["ClockBoard", "UpperPage"] // , "AppKit"]
const component_keep_alive = ["DemoPage"]
const component_disable_debug = [] // ["DemoPage"]
const component_time = {
  ClockBoard: 30000,
  AppKit: 3000
}
const curComp = reactive({
  compIndex: 0,
  compName: component_list[0],
  enable_debug: computed(() => !component_disable_debug.includes(curComp.compName)),
  stopAutoSwitch: false
})

// 初始化计时器
const timer = reactive({
  timerId: null,
  timerTime: computed(() => {
    return component_time[curComp.compName] || 10000
  })
})

// 加载所有组件
const loadAllComponents = () => {
  for (let i = 0; i < component_list.length; i++) {
    const compName = component_list[i]
    components.value.set(i, defineAsyncComponent(() => import(`./components/${compName}.vue`)))
  }
}

const switchComp = (t="increase") => {
  const num = t === "increase" ? 1 : -1
  curComp.compIndex = (curComp.compIndex + component_list.length + num) % component_list.length
  curComp.compName = component_list[curComp.compIndex]
  if (!curComp.stopAutoSwitch) {
    clearInterval(timer.timerId)
    timer.timerId = setTimeout(switchComp, timer.timerTime)
  }
}

const stopAutoSwitch = () => {
  if (curComp.stopAutoSwitch) {
    curComp.stopAutoSwitch = false
    timer.timerId = setInterval(switchComp, timer.timerTime)
  } else {
    curComp.stopAutoSwitch = true
    clearInterval(timer.timerId)
  }
}

// const debug = ref(null)
// https://github.com/vueuse/vueuse/blob/main/packages/core/useSwipe/demo.vue
// const { isSwiping, direction } = useSwipe(debug, {
//   onSwipeEnd: () => {
//     if (direction.value === 'left') {
//       switchComp("increase")
//     } else if (direction.value === 'right') {
//       switchComp("decrease")
//     }
//   }
// })

// 加载所有组件
loadAllComponents()

// 自动切换组件
timer.timerId = setInterval(switchComp, timer.timerTime)

</script>

<template>
  <div class="main-container">
    <!-- <transition name="fade">
      <component :is="components.get(curComp.compIndex)" class="board"></component>
    </transition> -->
    <KeepAlive :include="component_keep_alive">
      <component :is="components.get(curComp.compIndex)" class="board"></component>
    </KeepAlive>
  </div>
  <div class="debug auto-flex hide-debug" v-if="curComp.enable_debug" ref="debug">
    <button @click="switchComp('decrease')"> debug1 </button>
    <button @click="stopAutoSwitch"> debug2 </button>
    <button @click="switchComp('increase')"> debug3 </button>
  </div>
  <div class="options">
    <span class="lock" v-if="curComp.stopAutoSwitch">lock</span>
  </div>
</template>

<style scoped>
.main-container {
  overflow: scroll;
  max-height: 100vh;
  background-color: black;
}

/* .board {
  transition: all 1s ease-in-out;
} */

/* 隐藏滚动条 */
.main-container::-webkit-scrollbar {
  display: none;
}

/*
  进入和离开动画可以使用不同
  持续时间和速度曲线。
*/
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.options {
  position: absolute;
  bottom: 0;
  /* right: 0; */
  padding: 1rem;
  display: flex;
  gap: 1rem;
  justify-content: flex-end;
  align-items: center;
}

</style>
