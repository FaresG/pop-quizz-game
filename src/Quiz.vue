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
const currentQuestion = ref(questions[currentQuestionIndex.value])
const correctAnswers = ref(0)
const selectedOption = ref()
const quizzDone = computed(() => currentQuestionIndex.value === questions.length - 1)
const submit = () => {
  if (!selectedOption.value) return
  if (selectedOption.value === currentQuestion.value.answer)
    correctAnswers.value++

  // go next question or show result
  if (quizzDone.value) showResult()
  else currentQuestionIndex.value++
}
const showResult = () => {

}
</script>
<template>
  <div class="min-w-[50vw] border rounded-2xl p-10 flex flex-col gap-10">
    <p class="text-2xl border-b-2 border-b-white py-2">{{ currentQuestion.question }}</p>
    <p
        v-show="quizzDone"
        class="text-xl text-yellow-700 text-center"
    >Result: {{ correctAnswers }} of {{ questions.length }} are correct!</p>
    <ul
        v-show="!quizzDone"
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
      <p class="p-4 text-center basis-4/5 bg-green-500 border-green-500 border rounded-2xl">Your answer is ...</p>
      <button
          class="basis-1/5 p-4 border rounded-2xl bg-blue-500 border-blue-500"
          @click="submit"
      >Submit</button>
    </div>
  </div>
</template>
