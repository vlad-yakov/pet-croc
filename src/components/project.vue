<template>
  <!--окно создания проекта-->

  <v-dialog v-model="dialog" max-width="500">
    <v-card>
      <v-container>
        <v-form @submit.prevent="addEvent">
          <!--Кнопка создания события у администратора проекта-->
          <v-select
            v-model="select"
            :names="names"
            name-text="name"
            label="ФИО"
            return-object
            single-line
          >
          </v-select>
          <v-text-field
            v-model="start"
            type="date"
            label="Начало*"
          ></v-text-field>
          <v-text-field
            v-model="end"
            type="date"
            label="Окончание*"
          ></v-text-field>
          <!--<v-text-field
                v-model="color"
                type="color"
                label="Нажмите, чтобы открыть палитру"
              ></v-text-field>-->
          <v-btn
            type="submit"
            color="#00A460"
            class="mr-4"
            @click.stop="dialog = false"
          >
            Создать
          </v-btn>
        </v-form>
      </v-container>
    </v-card>
  </v-dialog>
</template>

<script>
import Vue from "vue";

export default Vue.component("project", {});
</script>

<style lang="scss" scoped></style>
