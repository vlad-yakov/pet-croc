<template>
  <v-app>
    <v-app-bar app>
      <header />
    </v-app-bar>
    <v-main>
      <graph />
    </v-main>
  </v-app>
</template>

<script>
import header from "./components/header";
import graph from "./components/graph";

export default {
  name: "App",
  data: () => ({}),
  components: {
    header,
    graph,
  },
};
</script>
