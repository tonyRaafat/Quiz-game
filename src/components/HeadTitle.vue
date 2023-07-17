<script setup>
const { category } = defineProps(['category'])
</script>

<template>
  <div class="head">
    <h1 class="title">{{ category }} QUIZ</h1>
    <img class="globe-logo" src="../assets/globe-img.svg" alt="" />
  </div>
</template>

<style scoped>
.head {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: end;
}
.title {
  font-size: clamp(0.5rem, 0.3rem + 2.4vw, 1.5rem);
  color: #f2f2f2;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
  text-transform: uppercase;
}
.globe-logo {
  position: relative;
  bottom: -35px;
  width: 132px;
  height: 106px;
  z-index: 2;
}
</style>
