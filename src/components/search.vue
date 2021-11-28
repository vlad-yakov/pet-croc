<template>
  <div class="search">
    <template>
      <v-btn color="#00A460" dark @click.stop="search = !search">
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </template>

    <v-navigation-drawer v-model="search" temporary :right="right">
      <v-input :names="[]" :projects="[]" width="90px"></v-input>
      <v-btn dark color="#00A460">
        <v-text> Найти </v-text>
      </v-btn>
      <hr />
      <h3>Мои проекты:</h3>

      <v-list dense> </v-list>
    </v-navigation-drawer>
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
