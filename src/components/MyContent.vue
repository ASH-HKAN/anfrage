<template>
  <div>
    <b-jumbotron>
      <template></template>

      <b-list-group>
        <b-list-group-item variant="warning">{{
          cQuestion.question
        }}</b-list-group-item>
      </b-list-group>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          variant="secondary"
          v-for="(answer, i) in answers"
          :key="i"
          @click="changeindex(i)"
          :class="[!answered && cindex === i ? 'selected' : '']"
          >{{ answer }}
        </b-list-group-item>
      </b-list-group>
      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="cindex === null || answered"
        href="#"
        >Submit</b-button
      >
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

 <script>
import _ from "lodash";
export default {
  props: {
    cQuestion: Object,
    next: Function,
    increment: Function,
  },

  computed: {
    answers() {
      let answers = [...this.cQuestion.incorrect_answers];
      answers.push(this.cQuestion.correct_answer);
      return answers;
    },
  },

  data() {
    return {
      cindex: null,
      shuffledAnswers: [],
      correctAnswer: null,
      answered: false,
    };
  },

  watch: {
    cQuestion: {
      immediate: true,
      handler() {
        this.cindex = null;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    changeindex(i) {
      this.cindex = i;
    },

    shuffleAnswers() {
      let answers = [
        ...this.cQuestion.incorrect_answers,
        this.cQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      console.log(this.shuffledAnswers);
      this.correctAnswer = this.shuffledAnswers.indexof(
        this.cQuestion.correct_answer
      );
    },

    submitAnswer() {
      let is_correct = false;
      if (this.cindex === this.correctAnswer) {
        is_correct = true;
      }
      (this.answered = true), this.increment(is_correct);
    },
  },
};
</script>


<style scoped>
.list-group-item-secondary:hover {
  background-color: #eee;
  cursor: pointer;
}
.btn {
  margin: 10px 2px;
}
.selected {
  background-color: grey;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
</style>