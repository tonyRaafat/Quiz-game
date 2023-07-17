<script async setup>
import { ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import Results from '../components/Results.vue'
import Questions from '../components/Questions.vue'

const route = useRoute()
const router = useRouter()

let { difficulty, categoryId } = route.params
let showResults = ref(false)
let score = 0
let questions

try {
  let response = await fetch(
    `https://opentdb.com/api.php?amount=10&category=${categoryId}&difficulty=${difficulty}&type=multiple`
  )
  let jsonRespone = await response.json()
  if (jsonRespone.response_code == 0) {
    questions = jsonRespone.results
    questions.forEach((element) => {
      element.selected = false
    })
    console.log(questions)
  } else {
    alert('No Questions Found')
    router.push('/')
  }
} catch (error) {
  console.log('Error', error)
  router.push('/')
}
</script>

<template>
  <Questions
    v-if="!showResults"
    :questions="questions"
    @toShow="
      (passedData) => {
        showResults = passedData.toShowResults
        score = passedData.score
      }
    "
  />
  <Results v-else :score="score" />
</template>

<style scoped></style>
