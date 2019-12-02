<template>
  <div class="quiz">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue';
import QuestionBox from '@/components/QuestionBox.vue';

export default {
  name: 'Quiz',
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next() {
      this.index = this.index + 1;
    },
    increment(isCorrenct) {
      console.log('janusz');
      if (isCorrenct) {
        this.numCorrect = this.numCorrect + 1;
      }
      this.numTotal = this.numTotal + 1;
    },
  },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get',
    }).then(resp => resp.json())
      .then((jsonData) => {
        this.questions = jsonData.results;
        console.log(this.questions);
      });
  },
};
</script>
