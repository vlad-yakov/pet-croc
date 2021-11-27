<template>
  <!--окно создания дежурства-->

  <v-dialog v-model="createDuty" max-width="500">
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
import db from "@/main";

export default Vue.component("duty", {
  methods: {
    async addEvent() {
      if (this.name && this.start && this.end) {
        await db.collection("calEvent").add({
          name: this.name,
          details: this.details,
          start: this.start,
          end: this.end,
          color: this.color,
        });
        this.getEvents();
        (this.name = ""),
          (this.details = ""),
          (this.start = ""),
          (this.end = ""),
          (this.color = "");
      } else {
        alert("Вам необходимо ввести имя события, время его начала и конца");
      }
    },
    editEvent(ev) {
      this.currentlyEditing = ev.id;
    },
    async updateEvent(ev) {
      await db.collection("calEvent").doc(this.currentlyEditing).update({
        details: ev.details,
      });
      (this.selectedOpen = false), (this.currentlyEditing = null);
    },
    async deleteEvent(ev) {
      await db.collection("calEvent").doc(ev).delete();
      (this.selectedOpen = false), this.getEvents();
    },
    showEvent({ nativeEvent, event }) {
      const open = () => {
        this.selectedEvent = event;
        this.selectedElement = nativeEvent.target;
        setTimeout(() => (this.selectedOpen = true), 10);
      };
      if (this.selectedOpen) {
        this.selectedOpen = false;
        setTimeout(open, 10);
      } else {
        open();
      }
      nativeEvent.stopPropagation();
    },
    updateRange({ start, end }) {
      this.start = start;
      this.end = end;
    },
  },
});
</script>

<style lang="scss" scoped></style>

<v-menu
  v-model="selectedOpen"
  :close-on-content-click="false"
  :activator="selectedElement"
  offset-x
>
            <v-card color="grey lighten-4" min-width="350px" flat>
              <v-toolbar :color="selectedEvent.color" dark>
                <v-btn icon>
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
                <v-toolbar-title v-html="selectedEvent.name"></v-toolbar-title>
                <v-spacer></v-spacer>
                <v-btn icon>
                  <v-icon>mdi-heart</v-icon>
                </v-btn>
                <v-btn icon>
                  <v-icon>mdi-dots-vertical</v-icon>
                </v-btn>
              </v-toolbar>
              <v-card-text>
                <span v-html="selectedEvent.details"></span>
              </v-card-text>
              <v-card-actions>
                <v-btn text color="secondary" @click="selectedOpen = false">
                  Cancel
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-menu>
