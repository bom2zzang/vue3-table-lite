<template>
  <div>
    <ul>
      <li v-for="(d, i) in  data" :key="i">
        {{d.item}}
      </li>
    </ul>
  </div>
</template>

<script>
import { defineComponent } from "vue";


export default defineComponent({
  name: "row-detail-view",
  props: {
    data:{
      type: Object,
      require: false
    }

  }
});
</script>

<style scoped></style>
