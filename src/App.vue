<template>
  <div class="logo__container">
    <img
      class="logo"
      src="https://files.cdn.printful.com/upload/og-image-tag/d6/d62afc71a280b36ea96a3702502eacec_l?v=1576220517"
      alt="Printful logo"
    />
  </div>

  <form
    class="form__container"
    @submit.prevent="formData"
    v-if="whatToShow === 'start'"
  >
    <label for="name"> Write your name</label>
    <input
      class="form__input__name"
      type="text"
      placeholder="Your name"
      name="name"
      id="name"
      v-model="formUsername"
    />

    <label for="test">Choose your test</label>
    <select
      class="form__select"
      id="test"
      name="testCategory"
      v-model="formCategory"
    >
      <option v-for="{ title, id } in quizzAllNames" :key="id" :value="id">
        {{ title }}
      </option>
    </select>
    <Button text="Start" type="submit" />
  </form>

  <div class="question__container" v-else-if="whatToShow === 'questions'">
    <h1>Question {{ quizzQuestionId + 1 }}</h1>
    <div v-for="{ title, id } in quizzQuestion" :key="id">
      <h3>{{ title }}</h3>
    </div>
    <div class="all__option__container">
      <div
        class="one__option__container"
        v-for="{ title, id } in quizzAnswers"
        :key="id"
      >
        <button
          class="option__button"
          @click="choosenAnswer(id)"
          v-bind:class="{ choosen: this.answer === id }"
        >
          {{ title }}
        </button>
      </div>
    </div>

    <Button text="Next question" @click="showSingleQuestion" />
  </div>

  <div class="results__container" v-else-if="whatToShow === 'results'">
    <h1>Thanks, {{ playerName }} for playing!</h1>
    <p>
      You got {{ howManyCorrect }} points, out of
      {{ quizzAllQuestions.length }} possible!
    </p>
    <Button text="Start again" @click="startAgain" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import Button from "./components/Button.vue";

type quizzType = {
  id: number;
  title: string;
};

export default defineComponent({
  name: "Home",
  components: {
    Button,
  },
  data: () => ({
    quizzAllNames: [] as quizzType[],
    quizzAllQuestions: [] as quizzType[],
    quizzAnswers: [] as quizzType[],
    quizzQuestion: [] as quizzType[],
    answerArray: [] as string[],
    quizzQuestionId: 0,
    answer: "",
    quizzAnswerId: "",
    formCategory: "",
    formUsername: "",
    playerName: "",
    correctAnswerString: "",
    howManyCorrect: "",
    whatToShow: "start",
  }),
  methods: {
    async getQuestions() {
      await axios
        .get(
          `https://printful.com/test-quiz.php?action=questions&quizId=${this.formCategory}`
        )
        .then((res) => {
          this.quizzAllQuestions = res.data;
        });
    },
    getAnswers() {
      axios
        .get(
          `https://printful.com/test-quiz.php?action=answers&quizId=${this.formCategory}&questionId=${this.quizzAnswerId}`
        )
        .then((res) => {
          this.quizzAnswers = res.data;
        });
    },
    async formData() {
      if (this.formCategory === "" || this.formUsername === "") {
        alert("Please fill both fields");
      } else {
        this.whatToShow = "questions";
        await this.getQuestions();
        this.playerName = this.formUsername;
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
          this.whatToShow = "results";
          this.getCorrectAnswers();
        } else {
          this.quizzQuestionId = this.quizzQuestionId + 1;

          this.quizzQuestion = [this.quizzAllQuestions[this.quizzQuestionId]];

          this.quizzAnswerId = this.quizzQuestion[0].id.toString();
          this.getAnswers();
          this.submitAnswer();
        }
      }
    },
    choosenAnswer(answer: string) {
      this.answer = answer;
    },
    submitAnswer() {
      if (this.answer === "") {
        alert("Choose answer");
      } else {
        this.answerArray.push(this.answer);
        this.answer = "";
        console.log(this.answerArray);
      }
    },
    getCorrectAnswers() {
      this.showCorrect();
      axios
        .get(
          `https://printful.com/test-quiz.php?action=submit&quizId=${this.formCategory}${this.correctAnswerString}`
        )
        .then((res) => {
          this.howManyCorrect = res.data.correct;
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
  },
  async created() {
    await axios
      .get("https://printful.com/test-quiz.php?action=quizzes")
      .then((res) => {
        this.quizzAllNames = res.data;
      });
  },
});
</script>
<style lang="scss">
* {
  font-size: 20px;
}

.logo__container {
  display: flex;
  justify-content: center;
}

.logo {
  width: 350px;
}

.form__container,
.question__container,
.results__container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.form__input__name {
  width: 245px;
  font-size: 20px;
  padding: 2px 0px;
  padding-left: 5px;
  border: 1px solid black;
  border-radius: 6px;
}

.form__select {
  width: 250px;
  font-size: 20px;
  padding: 2px 0px 2px 3px;
  border: 1px solid black;
  border-radius: 6px;
}

.option__button {
  padding: 5px 20px;
  background-color: #0c7c59;
  border: none;
  border-radius: 6px;
  color: white;
  cursor: pointer;
  font-size: 20px;
  width: 200px;
  &:hover {
    background-color: #b8311c;
    transition: 0.3s;
  }
}

.all__option__container {
  display: grid;
  grid-template-columns: 200px 200px;
  grid-gap: 20px;
  justify-items: center;
}

.choosen {
  background-color: burlywood;
}

.one__option__container {
  display: flex;
}
</style>
