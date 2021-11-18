<template>
  <v-app>
    <v-app-bar app flat>
      <div class="d-flex align-center">
        <v-img
          alt="croc-name"
          contain
          min-width="100"
          src=".\assets\croc-name.svg"
          width="100"
        />
      </div>

      <div class="p_name">
        <v-toolbar-title>
          Создание мультиканального контакт-центра на российском решении Naumen
          для банка Нейва</v-toolbar-title
        >
      </div>

      <v-spacer></v-spacer>

      <v-btn href="" target="_blank" color="#00A460">
        <v-icon color="white"> mdi-file-plus-outline </v-icon>
      </v-btn>

      <v-btn href="" target="_blank" color="#00A460">
        <v-icon color="white"> mdi-file-outline color="black" </v-icon>
      </v-btn>

      <v-btn href="" target="_blank" color="#00A460">
        <v-icon color="white"> mdi-bell color="black" </v-icon>
      </v-btn>

      <v-btn href="" target="_blank" color="#00A460">
        <v-icon color="white"> mdi-magnify color="black" </v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <graph />
    </v-main>
  </v-app>
</template>

<script>
'import header from "./components/header.vue";';
import graph from "./components/graph";

export default {
  name: "App",
  data: () => ({}),
  сomponents: {
    graph,
  },
};
</script>

<style lang="scss" scoped>
.v-btn {
  margin: 0.3em;
}

.p_name {
  margin: 1em;
  max-width: 50em;
  overflow: auto;
  word-break: break-all;
  hyphens: auto;
}
</style>
