<template>
  <div v-if="takingQuiz">
    <h1>Quiz</h1>
    <h3>{{ `Question ${q + 1} of ${questions.length}` }}</h3>
    <p>{{ `${questions[q].text}` }}</p>
    <section v-for="ans of questions[q].answers" :key="ans">
      <input type="radio" :value="ans" v-model="answer" />{{ ans }}
    </section>
    <br />
    <button @click="nextQuestion">Save and Proceed</button>
  </div>

  <div v-else>
    <h1>Results</h1>
    <section v-for="(result, i) of results" :key="i">
      <h3>{{ `Question ${i + 1}` }}</h3>
      <p>
        <b>{{ `Question: ${result.q}` }}</b>
      </p>
      <p :style="correctness(i, result.a)">
        {{ `Your Answer: ${result.a}` }}
      </p>
    </section>
    <button @click="restart">Restart the Quiz</button>
  </div>
</template>

<script>
export default {
  props: {
    questions: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      takingQuiz: true,
      q: 0,
      answer: "",
      results: [],
    };
  },
  methods: {
    nextQuestion() {
      if (this.answer === "") {
        alert("You must select one answer");
        return;
      }
      this.results.push({
        q: this.questions[this.q].text,
        a: this.answer,
      });
      if (++this.q >= this.questions.length) {
        this.takingQuiz = false;
      }
      this.answer = "";
    },
    restart() {
      this.takingQuiz = this.questions.length >= 1;
      this.q = 0;
      this.answer = "";
      this.results = [];
    },
    correctness(q, a) {
      let color = '';
      switch(q) {
        case 2:
          color = a === 'John Adams' ? 'Green': 'Red';
          break;
        case 3:
          color = a === 'Paris' ? 'Green': 'Red';
          break;
        case 4:
          color = a === '26' ? 'Green': 'Red';
          break;
        default:
          color = 'Black';
          break;
      }
      return `color:${color};`;
    }
  },
  mounted() {
    this.takingQuiz = this.questions.length >= 1;
  },
};
</script>
