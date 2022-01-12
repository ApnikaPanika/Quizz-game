<template>
  <div class="logo__container">
    <img
      class="logo"
      src="https://files.cdn.printful.com/upload/og-image-tag/d6/d62afc71a280b36ea96a3702502eacec_l?v=1576220517"
      alt="Printful logo"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Logo",
});
</script>
<style lang="scss">
.logo__container {
  display: flex;
  justify-content: center;
}

.logo {
  width: 30%;
}
</style>
