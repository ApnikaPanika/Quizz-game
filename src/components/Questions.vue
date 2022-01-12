<template>
  <div class="question__container">
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
          v-bind:class="{ choosen: this.answer === id.toString() }"
        >
          {{ title }}
        </button>
      </div>
    </div>

    <Button text="Next question" @click="showSingleQuestion" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Button from "../components/Button.vue";

export default defineComponent({
  name: "Question",
  components: {
    Button,
  },
  props: {
    quizzQuestionId: {
      type: Number,
    },
    quizzQuestion: {
      type: Array,
    },
    quizzAnswers: {
      type: Array,
    },
    choosenAnswer: {
      type: Function,
    },
    answer: {
      type: String,
    },
    showSingleQuestion: {
      type: Function,
    },
  },
});
</script>
<style lang="scss">
.question__container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.all__option__container {
  display: grid;
  grid-template-columns: 200px 200px;
  grid-gap: 20px;
  justify-items: center;
}

.one__option__container {
  display: flex;
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

.choosen {
  background-color: burlywood;
}
</style>
