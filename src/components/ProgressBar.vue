<template>
  <div class="progress__bar__container">
    <div class="progress__bar">
      <div
        class="progress__bar__filling"
        v-bind:style="{ width: procentageBarFill + '%' }"
      >
        {{ procentageBarFill }} %
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "ProgressBar",
  props: {
    procentageBarFill: { type: Number },
  },
});
</script>
<style lang="scss">
.progress__bar__container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.progress__bar {
  width: 50%;
  height: 20px;
  border-radius: 6px;
  border: none;
  background-color: rgb(189, 189, 189);
}

.progress__bar__filling {
  display: flex;
  justify-content: center;
  color: white;
  font-weight: bold;
  height: 20px;
  border-radius: 6px;
  border: none;
  background-color: #0c7c59;
  transition: 1.3s;
}
</style>
