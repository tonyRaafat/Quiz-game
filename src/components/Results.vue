<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()
const { score } = defineProps(['score'])
</script>

<template>
  <main>
    <img src="../assets/results-logo.svg" alt="" />
    <div class="res-cont">
      <h1>Results</h1>
      <h3>
        you got <span class="score">{{ score }}</span> question{{ score > 1 ? 's' : '' }} right
      </h3>
    </div>
    <button @click="() => router.push('/')" class="try-again-btn">try again</button>
  </main>
</template>

<style scoped>
* {
  color: #1d355d;
}
.res-cont {
  text-align: center;
  width: 100%;
}
main {
  background: rgb(255, 255, 255);
  width: 100%;

  /* width: 95vw; */
  /* max-width: 464px; */
  max-height: 515px;
  border-radius: 24px;
}

main > * {
  text-align: center;
  padding-bottom: 8px;
  padding-top: 8px;
  width: 90%;
}

h1 {
  color: #1d355d;
  font-weight: 700;
}
h3 {
  font-weight: 400;
}
.score {
  color: rgb(57, 247, 57);
  font-weight: 700;
  text-align: center;
  font-size: larger;
}
.try-again-btn {
  text-align: center;
  border-color: #2f527b;
  background-color: rgb(223, 223, 223);
  border: 1px solid black;
  border-radius: 15px;
  width: 90%;
  height: 56px;
  margin: 20px 10px;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
}

@media only screen and (max-width: 768px) {
  /* For mobile phones: */
  .main {
    margin-left: 10px;
    margin-right: 10px;
    /* width: 95vw; */
  }
}
</style>
