<template>
  <div>
    <ul>
      {{
        data
      }}
    </ul>
  </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "row-detail-view",
  props: {
    data: {
      type: String,
      require: false,
    },
  },
});
</script>

<style scoped></style>
