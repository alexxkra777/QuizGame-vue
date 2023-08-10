<template>
    <div>
        <p class="text-bold text-3xl">
            Question {{ currentQuestion }} of {{ totalQuestions }}
        </p>
        <p class="text-bold text-3xl text-center">
            {{ countTimer }}
        </p>
        <p class="text-bold text-3xl">
            {{ currentQuestionText }}
        </p>
        <div class="grid grid-cols-1 gap-4">
            <button v-for="(item, index) in options" :key="index" @click="selectAnswer(item.isCorrect, item.answer)"
                class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2 focus:bg-gray-500">
                {{ item.answer }}
            </button>
        </div>
        <div>
            <button v-show="currentQuestion < totalQuestions" @click="nextQuestion"
                class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2 m-5">
                Next
            </button>
            <button v-show="currentQuestion === totalQuestions" @click="displayResult"
                class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2 m-5">
                Submit
            </button>
        </div>
    </div>
</template>
  
<script>
import { computed } from 'vue';
import { shuffle } from 'lodash';

export default {
    name: 'QuestionView',
    props: {
        currentQuestion: Number,
        totalQuestions: Number,
        countTimer: Number,
        questions: Array,
    },
    emits: ['selectAnswer', 'nextQuestion', 'displayResult'],
    setup(props, { emit }) {
        const currentQuestionText = computed(() => props.questions[props.currentQuestion - 1].question);
        const options = computed(() => shuffle(props.questions[props.currentQuestion - 1].options));

        const selectAnswer = (isCorrect, answer) => {
            if (isCorrect) {
                emit('selectAnswer', answer);
            }
        };

        const nextQuestion = () => {
            emit('nextQuestion');
        };

        const displayResult = () => {
            emit('displayResult');
        };

        return {
            currentQuestionText,
            options,
            selectAnswer,
            nextQuestion,
            displayResult,
        };
    },
};
</script>
  