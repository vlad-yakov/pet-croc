<template>
  <div class="footer">
    <div class="d-flex align-center">
      <v-img />

      <v-img
        alt="croc-name"
        contain
        min-width="100"
        src=".\assets\croc-name.svg"
        width="100"
      />
    </div>

    <v-spacer>{{ "имя проекта" }}</v-spacer>

    <v-btn href="" target="_blank">
      <v-icon> mdi-magnify color="black" </v-icon>
    </v-btn>

    <v-btn href="" target="_blank">
      <v-icon> mdi-magnify color="black" </v-icon>
    </v-btn>

    <v-btn href="" target="_blank">
      <v-icon> mdi-magnify color="black" </v-icon>
    </v-btn>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
.v-btn {
  margin: 0.3em;
  color: "#00a460";

  &.v-icon {
    background-color: "#00a460";
  }
}
</style>
