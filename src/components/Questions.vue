<script setup>
import { onBeforeMount, ref, defineEmits } from 'vue'
import HeadTitle from './HeadTitle.vue'

const emits = defineEmits(['toShow'])
const { questions } = defineProps(['questions'])

let showResults = ref(false)
let answerSelected = ref(false)
let score = 0
let n = ref(0)
let correctAnswer
let incorrectAnswers
let number = 2
let choices = ref([])

function getRandom(myNumber) {
  let randomNum = Math.floor(Math.random() * 4)
  while (randomNum == myNumber) {
    randomNum = Math.floor(Math.random() * 4)
  }
  number = randomNum
}

function decodeHtml(html) {
  var txt = document.createElement('textarea')
  txt.innerHTML = html
  return txt.value
}

function getChoices() {
  incorrectAnswers = questions[n.value].incorrect_answers
  correctAnswer = questions[n.value].correct_answer
  getRandom(number)
  let j = 0
  for (let i = 0; i <= 3; i++) {
    if (i == number) {
      choices.value[i] = { chosenAnswer: decodeHtml(correctAnswer), selected: false }
    } else {
      choices.value[i] = { chosenAnswer: decodeHtml(incorrectAnswers[j]), selected: false }
      j++
    }
  }
}

function nextQuestion() {
  if (n.value < 9) {
    n.value++
    answerSelected.value = false
    getChoices()
  }
}

onBeforeMount(() => getChoices())
</script>

<template>
  <HeadTitle :category="questions[n].category" />

  <main v-if="questions">
    <h3 class="question">
      {{ decodeHtml(questions[n].question) }}
    </h3>
    <button
      v-for="(choice, index) in choices"
      :disabled="answerSelected"
      :key="index"
      ref="answer"
      @click="
        () => {
          choice.selected = true
          answerSelected = true
          showResults = choice.chosenAnswer !== correctAnswer
          if (choice.chosenAnswer === correctAnswer) {
            score++
          }
        }
      "
      :class="` answer ${answerSelected ? '' : 'hover'} ${
        choice.selected ? (choice.chosenAnswer == correctAnswer ? 'correct' : 'wrong') : ''
      } `"
    >
      {{ choice.chosenAnswer }}
    </button>
  </main>
  <button
    v-if="answerSelected"
    @click="
      () => {
        showResults ? emits('toShow', { toShowResults: showResults, score: score }) : nextQuestion()
      }
    "
    class="next"
  >
    {{ showResults ? 'Show Score' : 'Next Question' }}
  </button>
</template>

<style scoped>
main {
  background: rgb(255, 255, 255);
  width: 100%;
  padding-bottom: 10px;
  display: grid;
  place-items: center;
  border-radius: 24px;
}
.answer {
  font-size: clamp(0.5rem, 0.7rem + 3.5vw, 1.4rem);

  border-color: #2f527b;
  background-color: rgb(223, 223, 223);
  border: 1px solid black;
  border-radius: 12px;
  width: 90%;
  padding-top: 10px;
  padding-bottom: 10px;
  margin: 5px 10px;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
}
.hover:hover {
  background-color: #f9a826;
  color: white;
  border: none;
}
.correct {
  background-color: rgb(64, 204, 64);
  color: white;
  border: none;
}

.wrong {
  animation: wrongBlinker 1s ease-in-out infinite both;
  background-color: rgb(255, 43, 43);
  color: white;
  border: none;
}
.question {
  font-size: clamp(0.3rem, 0.6rem + 1vw, 1rem);
  display: flex;
  width: 100%;
  max-width: 460px;
  overflow-wrap: break-word;
  padding: 50px 6px 6px 5px;
  flex-direction: column;
  text-align: center;
  color: #2f527b;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
}
.question > * {
  word-break: break-all;
}
.next {
  text-align: center;
  /* color: #1d355d; */
  color: white;
  background-color: #0ed3ac;
  /* border: 1px solid black; */
  border: none;
  border-radius: 12px;
  width: 100%;
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
@media only screen and (max-width: 468px) {
  .main {
    margin-left: 10px;
    margin-right: 10px;
    overflow-y: scroll;
    max-height: calc(100vh - 80px);
  }
}
</style>
