<script setup>
import { onMounted, reactive, ref } from 'vue';
import axios from 'axios';

const container = ref(null);
const state = reactive({
  text: '',
  wallpaper: '../assets/img/wallpaper.jpg',
});

const getSentFromFreeApi = () => {
  axios.get('https://api.kanye.rest')
    .then((response) => {
      state.text = response.data.quote;
    })
    .catch((error) => {
      console.log(error);
    });
}

// 获取 bing 壁纸
const getBingWallpaper = () => {
  axios.get('https://cn.bing.com/HPImageArchive.aspx?format=js&idx=0&n=1')
    .then((response) => {
      const url = 'https://cn.bing.com' + response.data.images[0].url;
      container.value.style.backgroundImage = `url(${url})`;
    })
    .catch((error) => {
      console.log(error);
    });
}

onMounted(() => {
  getSentFromFreeApi();
  getBingWallpaper();
});

</script>

<template>
  <div class="fullscreen demo-container" ref="container">
    <p>{{ state.text }}</p>
  </div>
</template>

<style scoped>
.demo-container {
  flex-direction: column;
  font-size: 3rem;
  text-align: center;
  text-shadow: 0 0 10px var(--vt-c-black);
  color: var(--vt-c-white-soft);
  background-color: var(--vt-c-black);
  background-image: url('../assets/img/wallpaper.jpg');
  background-size: cover;
  font-weight: bold;
}

iframe {
  width: 100%;
  height: 100%;
  border: none;
}
</style>