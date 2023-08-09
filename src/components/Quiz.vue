<template>
  <div class="flex w-full h-screen justify-center items-center">

    <div v-if="game === 'main'">
      <div class="w-full max-w-xl p-3">
        <h1 class="font-bold text-5xl text-center text-indigo-700">QUIZ Game</h1>
        <button @click="startQuizGame()"
          class="bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2 m-2">Start
          Quiz</button>
      </div>
    </div>

    <div v-else-if="game === 'start'">
      <p class="text-bold text-3xl">
        Question {{ currentQuestion }} of {{ questions.length }}
      </p>
      <p class="text-bold text-3xl text-center">
        {{ countTimer }}
      </p>
      <p class="text-bold text-3xl">
        {{ questions[currentQuestion - 1].question }}
      </p>
      <div class="grid grid-cols-1 gap-4">
        <button type="button" v-for="(item, index) in options" :key="index"
          @click="correctAnswer(item.isCorrect, item.answer)"
          class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2 focus:bg-gray-500">
          {{ item.answer }}
        </button>
      </div>
      <div>
        <button v-show="currentQuestion < questions.length" @click="NextQuestion()"
          class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2 m-5">
          Next
        </button>
        <button v-show="currentQuestion === questions.length" @click="displayResult()"
          class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2 m-5">
          Submit
        </button>
      </div>
    </div>


    <div v-else-if="game === 'end'" class="w-full max-w-xl p-3">
      <p class="text-bold text-3xl">Thanks for playing</p>
      <p class="text-bold text-3xl">Your score is {{ points }}/{{ questions.length }}</p>
      <p class="text-bold text-3xl">Your time is {{ countTimer }} seconds</p>
      <p class="text-bold text-3xl">Your percentage success rate is {{ resultPercent }}%</p>
      <a href="/" class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2">Play
        Again</a>
    </div>
  </div>
</template>

<script>
import Data from "../assets/data.json";
import { shuffle } from 'lodash';

export default {
  name: "QuizGame",
  props: ["totalPoints", "totalQuestions"],
  data() {
    return {
      currentQuestion: 1,
      points: 0,
      answersArray: [],
      arr: 0,
      countTimer: 0,
      timer: null,
      game: "main",
      timerRunning: false,
      startQuiz: false,
      showResult: false,
      questions: Data,
      options: shuffle(Data[0].options)
    };
  },

  methods: {
    startQuizGame() {
      this.startQuiz = true;
      this.timerRunning = true;
      this.game = "start"
      this.countTimerFunc();
    },

    correctAnswer(isCorrect, answer) {
      if (isCorrect) {
        this.answersArray.push(answer);
        this.arr = new Set(this.answersArray);
      }
    },

    countTimerFunc() {
      this.timer = setTimeout(() => {
        if (this.timerRunning == true) {
          this.countTimer++;
          this.countTimerFunc();
        }
      }, 1000);
    },

    NextQuestion() {
      this.options = this.questions[this.currentQuestion].options;
      this.options = shuffle(this.options);
      this.currentQuestion += 1;
    },

    Percent() {
      this.percent = 3 / 100;
      this.resultPercent = this.points / this.percent;
    },

    displayResult() {
      this.showResult = true;
      this.game = "end"
      this.timerRunning = false;
      this.points = this.arr.size;
      this.Percent();
    },
  },

};
</script>
