<template>
  <div class="results__container">
    <h1>Thanks, {{ formUsername }} for playing!</h1>
    <p>
      You got {{ howManyCorrect }} points, out of
      {{ quizzAllQuestions.length }} possible!
    </p>
    <Button text="Start again" @click="startAgain" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Button from "../components/Button.vue";

export default defineComponent({
  name: "Results",
  components: {
    Button,
  },
  props: {
    startAgain: {
      type: Function,
    },
    formUsername: {
      type: String,
    },
    howManyCorrect: {
      type: String,
    },
    quizzAllQuestions: {
      type: Array,
    },
  },
});
</script>
<style lang="scss">
.results__container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}
</style>
