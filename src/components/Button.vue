<template>
  <div>
    <button class="button">{{ text }}</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Button",
  props: {
    text: { type: String },
  },
});
</script>
<style lang="scss">
* {
  font-size: 20px;
}

.button {
  padding: 5px 20px;
  background-color: #0c7c59;
  border: none;
  border-radius: 6px;
  color: white;
  cursor: pointer;
  font-size: 20px;

  &:hover {
    background-color: #b8311c;
    transition: 0.3s;
  }
}
</style>
