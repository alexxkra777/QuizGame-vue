<template>
    <div class="w-full max-w-xl p-3">
        <p class="text-bold text-3xl">Thanks for playing</p>
        <p class="text-bold text-3xl">Your score is {{ points }}/{{ totalQuestions }}</p>
        <p class="text-bold text-3xl">Your time is {{ countTimer }} seconds</p>
        <p class="text-bold text-3xl">Your percentage success rate is {{ resultPercent }}%</p>
        <button @click="restartGame"
            class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2">
            Play Again
        </button>
    </div>
</template>
  
<script>
import { computed } from 'vue';

export default {
    name: 'EndGameView',
    props: {
        points: Number,
        totalQuestions: Number,
        countTimer: Number,
    },
    emits: ['restartGame'],
    setup(props, { emit }) {
        const percent = 3 / 100;
        const resultPercent = computed(() => (props.points / percent).toFixed(2));

        const restartGame = () => {
            emit('restartGame');
        };

        return {
            resultPercent,
            restartGame,
        };
    },
};
</script>
  