<script setup>
import Question from "../components/Question.vue";
import QuizHeader from "../components/QuizHeader.vue";
import Result from "../components/Result.vue";
import { useRoute } from "vue-router";
import { computed, ref } from "vue";
import quizzes from "../data/quizzes.json";

const route = useRoute();
const quizId = parseInt(route.params.id);

const quiz = quizzes.find((q) => q.id === quizId);
const currentQuestionIndex = ref(0);
const numberOfCorrectAnswer = ref(0);
const showResult = ref(false);
//---------same word do with computed property---
const questionStatus = computed(
  () => `${currentQuestionIndex.value}/${quiz.questions.length}`
);

//-------computed property ends here------------

//----------------try with watch----------------------
// const questionStatus = ref(
//   `${currentQuestionIndex.value}/${quiz.questions.length}`
// );

// watch(
//   () => currentQuestionIndex.value,
//   () => {
//     questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`;
//   }
// );

// If you pass currentQuestionIndex.value directly, you're passing the current value at the time of setting up the watcher, not a reference to it. Vue needs to know which reactive dependency it should track, which requires a function.

// () => currentQuestionIndex.value is a getter function that Vue will call to get the current value of currentQuestionIndex.value. Vue will automatically track this dependency and re-run the watcher when currentQuestionIndex.value changes.

//---------try with watch end here -------
const barPercentage = computed(
  () => `${(currentQuestionIndex.value / quiz.questions.length) * 100}%`
);

const onOptionSelected = (isCorrect) => {
  if (isCorrect) {
    numberOfCorrectAnswer.value++;
  }
  if (quiz.questions.length - 1 === currentQuestionIndex.value) {
    showResult.value = true;
  }
  currentQuestionIndex.value++;
};
</script>

<template>
  <div>
    <QuizHeader
      :questionStatus="questionStatus"
      :barPercentage="barPercentage"
    />
    <div>
      <Question
        v-if="!showResult"
        :question="quiz.questions[currentQuestionIndex]"
        @selectedOption="onOptionSelected"
      />
      <Result
        v-else
        :quizQuestionLength="quiz.questions.length"
        :numberOfCorrectAnswer="numberOfCorrectAnswer"
      />
    </div>
    <!-- <button @click="currentQuestionIndex++">Next Question</button> -->
  </div>
</template>
