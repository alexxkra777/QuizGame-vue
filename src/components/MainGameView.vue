<template>
    <div class="w-full max-w-xl p-3">
        <h1 class="font-bold text-5xl text-center text-indigo-700">QUIZ Game</h1>
        <button @click="startQuizGame"
            class="bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2 m-2">Start Quiz</button>
    </div>
</template>
  
<script>

export default {
    name: 'MainGameView',
    props: {
        startQuizGame: Function,
    },
};
</script>
  