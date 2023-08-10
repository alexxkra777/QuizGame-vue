<template>
  <div class="flex w-full h-screen justify-center items-center">
    <MainGameView v-if="game === 'main'" :startQuizGame="startQuizGame" />
    <QuestionView v-else-if="game === 'start'" :currentQuestion="currentQuestion" :totalQuestions="questions.length"
      :countTimer="countTimer" :questions="questions" @selectAnswer="selectAnswer" @nextQuestion="nextQuestion"
      @displayResult="displayResult" />
    <EndGameView v-else-if="game === 'end'" :points="points" :totalQuestions="questions.length" :countTimer="countTimer"
      @restartGame="restartGame" />
  </div>
</template>

<script>
import { ref } from 'vue';
import Data from '../assets/data.json';

import MainGameView from './MainGameView.vue';
import QuestionView from './QuestionView.vue';
import EndGameView from './EndGameView.vue';

export default {
  name: 'QuizGame',
  setup() {
    const currentQuestion = ref(1);
    const points = ref(0);
    const answersArray = ref([]);
    const arr = ref(new Set());
    const countTimer = ref(0);
    const timer = ref(null);
    const game = ref('main');
    const timerRunning = ref(false);
    const startQuiz = ref(false);
    const questions = ref(Data);

    const startQuizGame = () => {
      startQuiz.value = true;
      timerRunning.value = true;
      game.value = 'start';
      countTimerFunc();
    };

    const selectAnswer = (answer) => {
      answersArray.value.push(answer);
      arr.value = new Set(answersArray.value);
    };

    const countTimerFunc = () => {
      timer.value = setTimeout(() => {
        if (timerRunning.value) {
          countTimer.value++;
          countTimerFunc();
        }
      }, 1000);
    };

    const nextQuestion = () => {
      currentQuestion.value += 1;
    };

    const displayResult = () => {
      game.value = 'end';
      timerRunning.value = false;
      points.value = arr.value.size;
    };

    const restartGame = () => {
      currentQuestion.value = 1;
      points.value = 0;
      answersArray.value = [];
      arr.value = new Set();
      countTimer.value = 0;
      timer.value = null;
      game.value = 'main';
      timerRunning.value = false;
      startQuiz.value = false;
    };

    return {
      currentQuestion,
      points,
      countTimer,
      game,
      timerRunning,
      startQuiz,
      questions,
      startQuizGame,
      selectAnswer,
      nextQuestion,
      displayResult,
      restartGame,
    };
  },
  components: {
    MainGameView,
    QuestionView,
    EndGameView,
  },
};
</script>
