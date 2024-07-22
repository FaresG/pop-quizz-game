<script setup>
import {computed, ref} from "vue";

const questions = [
  {
    question: "What is the capital of France?",
    options: ["London", "Berlin", "Paris", "Rome"],
    answer: "Paris",
  },
  {
    question: "Which planet is closest to the sun?",
    options: ["Earth", "Mars", "Venus", "Mercury"],
    answer: "Mercury",
  },
  // Add more questions as needed
];

const currentQuestionIndex = ref(0)
const currentQuestion = computed(() => {
  if (!quizzDone.value)
    return questions[currentQuestionIndex.value]

  // just to prevent errors
  else return questions[questions.length - 1]
})
const correctAnswers = ref(0)
const selectedOption = ref()
const quizzDone = computed(() => currentQuestionIndex.value === questions.length)
const currentAnswerIsCorrect = computed(() => currentQuestion.value.answer === selectedOption.value)
const currentQuestionDone = ref(false)
const submit = () => {
  if (!selectedOption.value) return

  if (currentAnswerIsCorrect.value)
    correctAnswers.value++

  currentQuestionDone.value = true
}

const next = () => {
  // reset stuff
  currentQuestionIndex.value++
  selectedOption.value = ''
  currentQuestionDone.value = false
}
</script>
<template>
  <p
      v-show="quizzDone"
      class="text-3xl text-center"
  >Your final score: {{ correctAnswers }} out of {{ questions.length }}</p>
  <div
      v-show="!quizzDone"
      class="min-w-[50vw] border rounded-2xl p-10 flex flex-col gap-10"
  >
    <p class="text-2xl border-b-2 border-b-white py-2">{{ currentQuestion.question }}</p>
    <ul
        class="flex flex-col gap-4 text-xl"
    >
      <li
          v-for="option in currentQuestion.options"
      >
        <input
            type="radio"
            name="answer"
            :value="option"
            v-model="selectedOption"
            class="mr-4"
        />
        <span>{{ option }}</span>
      </li>
    </ul>
    <div class="flex gap-5 py-4">
      <p
          v-show="currentQuestionDone"
          :class="{'bg-green-500 border-green-500': currentAnswerIsCorrect, 'bg-red-500 border-red-500': !currentAnswerIsCorrect}"
          class="p-4 text-center basis-4/5 border rounded-2xl "
      >Your answer is {{ currentAnswerIsCorrect ? 'Correct' : 'Wrong' }}</p>
      <button
          v-show="!quizzDone && !currentQuestionDone"
          class="basis-1/5 p-4 border rounded-2xl bg-blue-500 border-blue-500"
          @click="submit"
      >Submit</button>
      <button
          v-show="!quizzDone && currentQuestionDone"
          class="basis-1/5 p-4 border rounded-2xl bg-blue-500 border-blue-500"
          @click="next"
      >Next</button>
    </div>
  </div>
</template>
