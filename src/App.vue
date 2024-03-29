<template>
  <Logo />

  <Form
    class="font__size__20"
    v-if="whatToShow === 'start'"
    :quizzAllCategories="quizzAllCategories"
    :submitFunction="formData"
    :name-value="formUsername"
    :select-value="formCategory"
    @update:name-value="formUsername = $event"
    @update:select-value="formCategory = $event"
  />

  <div v-else-if="whatToShow === 'questions'">
    <Question
      class="font__size__20"
      :quizzQuestionId="quizzQuestionId"
      :quizzQuestion="quizzQuestion"
      :quizzAnswers="quizzAnswers"
      :choosenAnswer="choosenAnswer"
      :answer="answer"
      :showSingleQuestion="showSingleQuestion"
    />

    <ProgressBar
      class="font__size__20"
      :procentageBarFill="procentageBarFill"
    />
  </div>

  <Results
    class="font__size__20"
    v-else-if="whatToShow === 'results'"
    :formUsername="formUsername"
    :howManyCorrect="howManyCorrect"
    :quizzAllQuestions="quizzAllQuestions"
    :startAgain="startAgain"
  />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import Form from "./components/Form.vue";
import Results from "./components/Results.vue";
import Logo from "./components/Logo.vue";
import Question from "./components/Questions.vue";
import ProgressBar from "./components/ProgressBar.vue";

type quizzType = {
  id: number;
  title: string;
};

export default defineComponent({
  name: "Home",
  components: {
    Form,
    Results,
    Logo,
    Question,
    ProgressBar,
  },
  data: () => ({
    quizzAllCategories: [] as quizzType[],
    quizzAllQuestions: [] as quizzType[],
    quizzAnswers: [] as quizzType[],
    quizzQuestion: [] as quizzType[],
    answerArray: [] as string[],
    quizzQuestionId: 0,
    answer: "",
    quizzAnswerId: "",
    formCategory: "",
    formUsername: "",
    correctAnswerString: "",
    howManyCorrect: "",
    whatToShow: "start",
    procentageBarFill: 0,
  }),
  methods: {
    async getQuestions() {
      await axios
        .get(
          `https://printful.com/test-quiz.php?action=questions&quizId=${this.formCategory}`
        )
        .then(({ data }) => {
          this.quizzAllQuestions = data;
        });
    },
    getAnswers() {
      axios
        .get(
          `https://printful.com/test-quiz.php?action=answers&quizId=${this.formCategory}&questionId=${this.quizzAnswerId}`
        )
        .then(({ data }) => {
          this.quizzAnswers = data;
        });
    },
    async formData() {
      if (this.formCategory === "" || this.formUsername === "") {
        alert("Please fill both fields");
      } else {
        this.whatToShow = "questions";
        await this.getQuestions();
        this.quizzQuestion = [this.quizzAllQuestions[0]];
        this.quizzAnswerId = this.quizzQuestion[0].id.toString();
        this.getAnswers();
      }
    },
    showSingleQuestion() {
      if (this.answer === "") {
        alert("Choose an answer");
      } else {
        if (this.quizzQuestionId === this.quizzAllQuestions.length - 1) {
          this.submitAnswer();
          this.whatToShow = "results";
          this.getCorrectAnswers();
        } else {
          this.quizzQuestionId = this.quizzQuestionId + 1;
          this.quizzQuestion = [this.quizzAllQuestions[this.quizzQuestionId]];
          this.quizzAnswerId = this.quizzQuestion[0].id.toString();
          this.getAnswers();
          this.submitAnswer();
          this.fillProgress();
        }
      }
    },
    choosenAnswer(answer: string) {
      this.answer = answer.toString();
    },
    submitAnswer() {
      if (this.answer === "") {
        alert("Choose answer");
      } else {
        this.answerArray.push(this.answer);
        this.answer = "";
      }
    },
    getCorrectAnswers() {
      this.showCorrect();
      axios
        .get(
          `https://printful.com/test-quiz.php?action=submit&quizId=${this.formCategory}${this.correctAnswerString}`
        )
        .then(({ data }) => {
          this.howManyCorrect = data.correct.toString();
        });
    },
    showCorrect() {
      this.correctAnswerString = this.answerArray
        .map((id) => `&answers[]=${id}`)
        .join("");
    },
    startAgain() {
      this.answerArray = [];
      this.quizzQuestionId = 0;
      this.whatToShow = "start";
    },
    fillProgress() {
      this.procentageBarFill = Math.round(
        (this.quizzQuestionId / this.quizzAllQuestions.length) * 100
      );
    },
  },
  async created() {
    await axios
      .get("https://printful.com/test-quiz.php?action=quizzes")
      .then(({ data }) => {
        this.quizzAllCategories = data;
      });
  },
});
</script>
<style lang="scss">
.font__size__20 {
  font-size: 20px;
}
</style>
