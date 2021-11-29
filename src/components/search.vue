<template>
  <div class="search">
    <template>
      <v-btn class="mrg" color="#00A460" dark @click.stop="search = !search">
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </template>

    <v-contaniner>
      <v-navigation-drawer
        app
        fixed
        floating
        v-model="search"
        absolute
        temporary
        right="true"
        width="440px"
        overlay-opacity="0"
      >
        <v-sheet>
          <v-row>
            <v-col cols="11"></v-col>
            <v-col cols="12" sm="9" offset="0px">
              <v-text-field
                dense
                v-model="find"
                clearable
                label="Найти проект"
                color="#00A460"
                id="poisk"
              ></v-text-field
              ><!--rules принимает аргументы --></v-col
            >

            <v-btn dark color="#00A460"> Найти </v-btn>
          </v-row>
        </v-sheet>
        <h3>Мои проекты:</h3>
        <div class="myProjects">
          <v-list-item v-for="pr in Projects" :key="pr.token"> </v-list-item
          ><!--нужно правильно назначить свойство объекта по которому итерируется массив -->
          <v-list-item-content>
            <v-list-item-title>Single-line item</v-list-item-title>
          </v-list-item-content>
        </div>
        <hr color="#00A460" />
        <div class="myProjects">
          <v-list-item v-for="pr in project" :key="pr.id"> </v-list-item>
          <!--нужно правильно назначить свойство объекта по которому итерируется массив -->
          <v-list-item-content>
            <v-list-item-title>Single-line item</v-list-item-title>
          </v-list-item-content>
        </div>
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
.mrg {
  margin: 5px;
}

.v-navigation-drawer {
  padding: 10px;
}

.v-text-field {
  margin: 15px;
}

.myProjects {
  margin: 5px 0px 5px 30px;
}

hr {
  margin: 15px;
}
h3 {
  padding: 7px 15px;
}
</style>
