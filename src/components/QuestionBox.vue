<template>
  <div class="question-box-container">
    <b-jumbotron>

      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{answer}}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#" @click="submitAnswer"
        :disabled="selectedIndex === null || answered">
        Submit
      </b-button>
      <b-button variant="success" @click="next">
        NExt
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  computed: {
    answers() {
      const answers = [...this.currentQuestion.incorrect_answers];
      return answers.concat(this.currentQuestion.correct_answer);
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
      console.log(index, 'index');
    },
    shuffleAnswers() {
      const answers = [...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = '';
      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected';
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct';
      } else if (this.answered && this.selectedIndex !== index && this.correctIndex !== index) {
        answerClass = 'incorrect';
      }
      return answerClass;
    },
  },
};
</script>

<style scoped>
  .list-group {
    margin-bottom: 20px;
  }
  .list-group-item:hover {
    background: #EEE;
    cursor: pointer;
  }
  .selected {
    background-color: lightblue!important;
  }
  .correct {
    background-color: green;
  }
  .incorrect {
    background-color: red;
  }
</style>
