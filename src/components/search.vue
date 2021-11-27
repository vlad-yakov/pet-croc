<template>
  <div class="search">
    <v-dialog
      height="500px"
      width="400px"
      transition="scroll-x-reverse-transition"
      origin="top left"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          v-bind="attrs"
          v-on="on"
          color="#00A460"
          @click.stop="search = true"
        >
          <v-icon color="white"> mdi-magnify </v-icon>
        </v-btn>
      </template>

      <v-card>
        <v-card-action>
          <v-input :names="[]" :projects="[]" width="90px"></v-input>
          <v-btn dark color="#00A460">
            <v-text> Найти </v-text>
          </v-btn>
          <h3>Мои проекты:</h3>
        </v-card-action>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import Vue from "vue";

export default Vue.component("v-search", {
  props: ["names", "projects"],
  data() {
    return {
      search: false,
    };
  },
});
</script>

<style lang="scss" scoped>
.v-dialog {
  position: absolute;
}

.v-btn {
  margin: 5px;
}
</style>
