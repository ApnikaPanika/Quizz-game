<template>
  <form class="form__container" @submit.prevent="submitFunction">
    <label for="name"> Write your name</label>
    <input
      class="form__input__name"
      type="text"
      placeholder="Your name"
      name="name"
      id="name"
      :value="nameValue"
      @input="$emit('update:nameValue', $event.target.value)"
    />

    <label for="test">Choose your test</label>
    <select
      class="form__select"
      id="test"
      name="testCategory"
      :value="selectValue"
      @change="$emit('update:selectValue', $event.target.value)"
    >
      <option v-for="{ title, id } in quizzAllCategories" :key="id" :value="id">
        {{ title }}
      </option>
    </select>
    <Button text="Start" type="submit" />
  </form>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Button from "../components/Button.vue";

export default defineComponent({
  name: "Form",
  components: {
    Button,
  },
  props: {
    quizzAllCategories: {
      type: Array,
    },
    submitFunction: {
      type: Function,
    },
    nameValue: {
      type: String,
    },
    selectValue: {
      type: String,
    },
  },
});
</script>
<style lang="scss">
.form__container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.form__input__name {
  font-size: 20px;
  width: 245px;
  padding: 2px 0px;
  padding-left: 5px;
  border: 1px solid black;
  border-radius: 6px;
}

.form__select {
  font-size: 20px;
  width: 250px;
  padding: 2px 0px 2px 3px;
  border: 1px solid black;
  border-radius: 6px;
}
</style>
