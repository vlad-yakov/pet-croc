<template>
  <v-app @wheel.prevent @touchmove.prevent @scroll.prevent>
    <v-main>
      <v-header></v-header>
      <graph></graph>
    </v-main>
  </v-app>
</template>

<script>
import header from "./components/header";
import graph from "./components/graph";

export default {
  name: "App",
  data: () => ({}),
  сomponents: {
    header,
    graph,
  },
};
</script>

<style lang="scss">
@font-face {
  font-family: "CrocRegular";
  src: url("./fonts/CrocRegular.eot");
  src: url("./fonts/CrocRegular.eot") format("embedded-opentype"),
    url("./fonts/CrocRegular.svg#CrocRegular") format("svg"),
    url("./fonts/CrocRegular.ttf") format("truetype");
}

.v-main {
  font-family: "CrocRegular";
}
</style>
