<script>
export default {
  data() {
    return {
      isStarted: false,
      isEnded: false,
      currentQuiz: 0,
      quizzes: [
        {
          question: "aaa",
          selections: [
            "aaa", "bbb", "ccc", "ddd"
          ],
          answer: 0
        },
        {
          question: "bbb",
          selections: [
            "bbb", "aaa", "ddd", "ccc"
          ],
          answer: 1
        }
      ],
      gotCorrect: [],
    }
  },
  methods: {
    answer(selection) {
      this.quizzes[this.currentQuiz].selections[this.quizzes[this.currentQuiz].answer] == selection ? this.gotCorrect[this.currentQuiz] = [true, selection] : this.gotCorrect[this.currentQuiz] = [false, selection];

      if (this.currentQuiz + 1 == this.quizzes.length) {
        this.isEnded = true;
        this.isStarted = false;
      } else {
        this.currentQuiz++
      }
    },
    restart() {
      this.isStarted = true;
      this.isEnded = false;
      this.currentQuiz = 0;
      this.gotCorrect = [];
    },
    home() {
      this.isStarted = false;
      this.isEnded = false;
      this.currentQuiz = 0;
      this.gotCorrect = [];
    }
  },
  mounted() {
    this.quizzes.map(_ => {
      this.gotCorrect.push([false]);
    });
  }
}
</script>

<template>
  <div v-if="!isStarted && !isEnded" class="animate__animated animate__rubberBand">
    <button
      class="bg-sky-400 px-6 py-4 rounded-lg text-sky-100 font-bold uppercase transition active:bg-sky-300 shadow-[0px_4px_rgb(2,132,199)] active:shadow-[0px_0px_rgb(2,132,199)] active:translate-y-1 text-center"
      @click="isStarted = true">start</button>
  </div>
  <div v-if="isStarted" class="text-center animate__animated animate__rubberBand">
    <p class="text-2xl font-bold">{{ quizzes[currentQuiz].question }}</p>
    <div class="grid grid-cols-2 gap-2 mt-6">
      <button v-for="selection, i in quizzes[currentQuiz].selections"
        @click="answer(selection)"
        class="bg-sky-400 px-6 py-4 rounded-lg text-sky-100 font-bold uppercase transition active:bg-sky-300 shadow-[0px_4px_rgb(2,132,199)] active:shadow-[0px_0px_rgb(2,132,199)] active:translate-y-1 text-center">
        {{ i + 1 }}. {{ selection }}
      </button>
    </div>
  </div>
  <div v-if="isEnded" class="text-center animate__animated animate__rubberBand">
    <p class="text-3xl">
      Correct: <span class="font-bold">{{ gotCorrect.filter(x => x == true).length }} /
        {{ quizzes.length }}</span>
    </p>
    <div class="mt-5 space-y-4">
      <div v-for="quiz, i in quizzes" class="text-left border-b-slate-400 border-b pb-3">
        <p class="text-xl">{{ i }}<span>.</span> {{ quiz.question }}</p>
        <p class="text-lg">Selections:</p>
        <ul class="grid grid-cols-2 gap-2">
          <li v-for="selection in quiz.selections"
            class="px-6 py-4 rounded-lg text-sky-100 font-bold uppercase transition text-center"
            v-bind:class="selection == gotCorrect[i][1] ? gotCorrect[i][0] ? 'bg-emerald-400 shadow-[0px_4px_rgb(4,120,87)]' : 'bg-red-400 shadow-[0px_4px_rgb(185,28,28)]' : 'bg-sky-400 shadow-[0px_4px_rgb(2,132,199)]'">
            {{ selection }}
          </li>
        </ul>
        <p class="text-xl mt-4 font-bold"
          v-bind:class="gotCorrect[i][0] ? 'text-emerald-700' : 'text-red-700'">
          {{ gotCorrect[i][0] ? 'Correct!' : 'Incorrect' }}
        </p>
        <div class="grid grid-cols-2 gap-4">
          <p class="text-xl">Answered:</p>
          <div
            class="inline-block bg-sky-400 px-6 py-4 rounded-lg text-sky-100 font-bold uppercase transition shadow-[0px_4px_rgb(2,132,199)] text-center">
            {{ gotCorrect[i][1] }}
          </div>
          <p class="text-xl">Answer:</p>
          <div
            class="inline-block bg-sky-400 px-6 py-4 rounded-lg text-sky-100 font-bold uppercase transition shadow-[0px_4px_rgb(2,132,199)] text-center">
            {{ quiz.selections[quiz.answer] }}
          </div>
        </div>
      </div>
      <button
        class="bg-emerald-400 shadow-[0px_4px_rgb(4,120,87)] px-6 py-4 rounded-lg text-sky-100 font-bold uppercase transition active:bg-sky-300 active:shadow-[0px_0px_rgb(2,132,199)] active:translate-y-1 text-center mr-3"
        @click="restart()">Restart</button>
      <button
        class="bg-sky-400 px-6 py-4 rounded-lg text-sky-100 font-bold uppercase transition active:bg-sky-300 shadow-[0px_4px_rgb(2,132,199)] active:shadow-[0px_0px_rgb(2,132,199)] active:translate-y-1 text-center"
        @click="home()">Home</button>
    </div>
  </div>
</template>
