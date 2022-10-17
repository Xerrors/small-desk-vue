<script setup>


</script>

<template>
  <div class="fullscreen auto-flex app-kit">
    <div class="first">
      <div>A</div>
      <div>B</div>
      <div>C</div>
    </div>
    <div class="second">
      <div>D</div>
      <div>E</div>
      <div>F</div>
    </div>
  </div>
</template>

<style scoped>
.app-kit {
  padding: 2rem;
  gap: 1.5rem;
}

.first, .second {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  gap: 1rem;
  width: 100%;
  height: 100%;
}

.first > div, .second > div {
  background-color: var(--vt-c-black);
  border-radius: 1rem;
  color: var(--vt-c-white);
  font-size: 3rem;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
}

.first > div:first-child {
  grid-column: 1 / 3;
}

.second > div:first-child {
  grid-row: 1 / 3;
}
</style>