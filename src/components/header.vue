<template>
  <v-app-bar app>
    <div class="d-flex align-center">
      <v-img
        alt="croc-name"
        contain
        min-width="100"
        src="..\assets\croc-name.svg"
        width="100"
      />
    </div>

    <v-spacer></v-spacer>

    <v-btn href="" target="_blank">
      <v-icon> mdi-magnify color="black" </v-icon>
    </v-btn>
  </v-app-bar>
</template>

<script>
import Vue from "vue";
export default Vue.component("header", {
  data: () => ({
    projectName: "Здесь имя проекта",
  }),
});
</script>
