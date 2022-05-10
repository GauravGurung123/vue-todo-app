<template>
  <router-view />
  <!-- <ToDoApp /> -->
</template>

<script>
import { defineComponent } from "vue";
// import ToDoApp from "components/ToDoApp.vue";

window.axios = require("axios");

export default defineComponent({
  name: "App",
  // components: {
  //   ToDoApp,
  // },
});
</script>
