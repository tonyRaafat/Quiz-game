<script setup>
import { ref, onBeforeMount } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

let isFetching = ref(false)
let categories = ref([])
let categoryId = 9
let diffculty = 'easy'

//function fetch categories
async function fetchCategories() {
  try {
    isFetching.value = true
    const response = await fetch('https://opentdb.com/api_category.php')
    const jsonRespone = await response.json()
    categories.value = await jsonRespone
    isFetching.value = false
  } catch (error) {
    console.log(error)
  }
}

function startGame() {
  router.push(`${diffculty}/${categoryId}/quiz`)
}
onBeforeMount(() => {
  fetchCategories() //call the function to get all available category data. This will be used in dropdown
})
</script>

<template>
  <main v-if="!isFetching">
    <select v-model="categoryId" class="categories form-select">
      <option class="" v-for="item in categories.trivia_categories" :value="item.id" :key="item.id">
        {{ item.name }}
      </option>
    </select>
    <div class="game-mode">
      <input type="radio" value="easy" v-model="diffculty" name="diffculty" id="easy" checked />
      <label for="easy">easy</label>
      <input type="radio" value="medium" v-model="diffculty" name="diffculty" id="medium" />
      <label for="medium">medium</label>
      <input type="radio" value="hard" v-model="diffculty" name="diffculty" id="hard" />
      <label for="hard">hard</label>
    </div>
    <button class="start-btn" @click="startGame">Start Game</button>
  </main>
</template>

<style scoped>
input[type='radio'] {
  display: none;
}

input[type='radio']:checked + label {
  background-color: #f9a826;
  color: white;
  border: none;
}

label {
  cursor: pointer;
  position: relative;
  border: 2px solid black;
  text-align: center;
}

main {
  background: rgb(255, 255, 255);
  width: 100%;
  padding-bottom: 10px;
  padding-top: 20px;
  display: grid;

  border-radius: 24px;
}
main > * {
  margin-top: 10px;
  margin-bottom: 10px;
}

.game-mode {
  display: grid;
  grid-auto-flow: column;
  grid-column: 1fr 1fr 1fr;
  width: 90%;
}
.game-mode * {
  margin: 5px;
  border-radius: 7px;
  color: #1d355d;
  font-weight: 500;
}
.categories {
  width: 90%;
  border: 1px solid black;
  color: #1d355d;
  font-weight: 500;
}
.start-btn {
  text-align: center;
  color: white;
  background-color: #0ed3ac;
  border: none;
  border-radius: 15px;
  width: 90%;
  height: 56px;
  margin: 20px 10px;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
}
.start-btn:hover {
  background-color: #0b707d;
  color: white;
  border: none;
}
</style>
