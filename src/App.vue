<script setup>
import { defineAsyncComponent, reactive, ref } from 'vue'

// 需要加载的组件集合
const components = ref(new Map())
const component_list = ["ClockBoard", "DemoPage", "UpperPage"]
const curComp = reactive({
  compIndex: 0,
  compName: component_list[0]

})

// 加载所有组件
const loadAllComponents = () => {
  for (let i = 0; i < component_list.length; i++) {
    const compName = component_list[i]
    components.value.set(i, defineAsyncComponent(() => import(`./components/${compName}.vue`)))
  }
}

const switchComp = () => {
  curComp.compIndex = (curComp.compIndex + 1) % component_list.length
}

// 加载所有组件
loadAllComponents()

// 每10秒切换一次组件
setInterval(() => {
  switchComp()
}, 10000)

const debug = () => {
  switchComp()
  console.log(curComp.compIndex)
}

</script>

<template>
  <div class="main-container">
    <transition name="fade">
      <component :is="components.get(curComp.compIndex)" class="board"></component>
    </transition>
  </div>
  <button class="debug" @click="debug">
    debug
  </button>
</template>

<style scoped>
.main-container {
  overflow: scroll;
  max-height: 100vh;
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

.debug {
  position: fixed;
  bottom: 0;
  right: 0;
  z-index: 999;
}
</style>
