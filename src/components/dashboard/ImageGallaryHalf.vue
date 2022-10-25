<script setup>
import { computed, reactive, ref } from 'vue';

const links = ref([
  {
    name: "duck",
    url: "https://i.gifer.com/XOsX.gif"
  }, // 鸭子
  {
    name: "squre",
    url: "https://i.gifer.com/3nR6.gif"
  },
  {
    name: "juan",
    url: "https://xerrors.oss-cn-shanghai.aliyuncs.com/imgs/20221025192943.png",
    type: 'photo'
  }
])


const switchLink = () => {
  curLink.value = (curLink.value + 1) % links.value.length
}

const state = reactive({
  curUrl: computed(() => {
    return links.value[curLink.value].url
  }),
  isFullSize: computed(() => {
    return links.value[curLink.value].type === 'photo'
  }),
})

const curLink = ref(0)


</script>

<template>
  <div class="image-gallary-half" @click="switchLink">
    <img :src="state.curUrl" alt="" :class="{'fullsize': state.isFullSize}">
  </div>
</template>

<style lang="less" scoped>
.image-gallary-half {
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  // border: 1px solid red;
}

img {
  width: 90%;
  height: 90%;
  object-fit: contain;
  // border: 1px solid red;
}

img.fullsize {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>