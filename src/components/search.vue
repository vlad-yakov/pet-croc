<template>
  <div class="search">
    <template>
      <v-btn color="#00A460" dark @click.stop="search = !search">
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </template>

    <v-contaniner>
      <v-navigation-drawer
        app
        bottom
        fixed
        floating
        v-model="search"
        absolute
        temporary
        right="true"
        color
        width="550px"
      >
        <v-sheet>
          <v-text-field
            filled
            dense
            v-model="find"
            clearable
            color="#00A460"
          ></v-text-field>
          <v-btn dark color="#00A460">
            <v-text> Найти </v-text>
          </v-btn>
        </v-sheet>
        <h3>Мои проекты:</h3>
        <hr />
        <v-list-item dense v-for="pr in project" :key="pr.id"> </v-list-item>
        <v-list-item-content>
          <v-list-item-title>Single-line item</v-list-item-title>
        </v-list-item-content>
      </v-navigation-drawer></v-contaniner
    >
  </div>
</template>

<script>
import Vue from "vue";

export default Vue.component("v-search", {
  props: ["names", "project"],
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

.mrg {
  margin: 15px;
}
.v-text-field {
  margin: 15px;
}

hr {
  margin: 15px;
}
h3 {
  padding: 7px 15px;
}
</style>
