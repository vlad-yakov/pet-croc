<template>
  <v-row class="fill-height">
    <v-col>
      <v-sheet height="64">
        <v-toolbar flat color="white">
          <v-btn fab text small @click="prev">
            <v-icon small>mdi-chevron-left</v-icon>
          </v-btn>

          <v-toolbar-title>{{ title }}</v-toolbar-title>

          <div class="createEv">
            <v-btn
              color="#00A460"
              dark
              @click.stop="dialog = true"
              margin="0.5em"
              >Создать событие</v-btn
            >
          </div>

          <div class="curWeek">
            <v-btn outlined class="md-3" @click="setToday" margin="0.5em">
              Текущая неделя
            </v-btn>
          </div>

          <div class="flex-grow-1"></div>
          <v-menu bottom right>
            <template v-slot:activator="{ on }">
              <v-btn outlined v-on="on">
                <span>{{ typeToLabel[type] }}</span>
                <v-icon right>mdi-menu-down</v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item @click="type = 'week'">
                <v-list-item-title>Неделя</v-list-item-title>
              </v-list-item>
              <v-list-item @click="type = 'month'">
                <v-list-item-title>Месяц</v-list-item-title>
              </v-list-item>
              <v-list-item @click="type = '4day'">
                <v-list-item-title>5/2</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
          <v-btn fab text small @click="next">
            <v-icon small>mdi-chevron-right</v-icon>
          </v-btn>
        </v-toolbar>
      </v-sheet>

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

      <v-dialog v-model="dialogDate" max-width="500">
        <!--создание события при нажатии на день в календаре-->
        <v-card>
          <v-container>
            <v-form @submit.prevent="addEvent">
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
                label="Начало"
              ></v-text-field>
              <v-text-field
                v-model="end"
                type="date"
                label="Окончание"
              ></v-text-field>
              <v-text-field
                v-model="color"
                type="color"
                label="Цвет (Нажмите, чтобы открыть палитру)"
              ></v-text-field>
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

      <v-sheet height="600">
        <v-calendar
          ref="calendar"
          v-model="focus"
          color="#00A460"
          :events="events"
          :event-color="getEventColor"
          :event-margin-bottom="3"
          :now="today"
          :type="type"
          @click:event="showEvent"
          @click:more="viewDay"
          @click:date="setDialogDate"
          @change="updateRange"
        >
        </v-calendar>
        <v-menu
          v-model="selectedOpen"
          :close-on-content-click="false"
          :activator="selectedElement"
          full-width
          offset-x
        >
          <v-card color="grey lighten-4" :width="350" flat>
            <v-toolbar :color="selectedEvent.color" dark>
              <v-btn @click="deleteEvent(selectedEvent.id)" icon>
                <v-icon>mdi-delete</v-icon>
              </v-btn>
              <v-toolbar-title v-html="selectedEvent.select"></v-toolbar-title>
              <div class="flex-grow-1"></div>
            </v-toolbar>

            <v-card-text>
              <form v-if="currentlyEditing !== selectedEvent.id">
                {{ selectedEvent.details }}
              </form>
              <form v-else>
                <textarea-autosize
                  v-model="selectedEvent.details"
                  type="text"
                  style="width: 100%"
                  :min-height="100"
                  placeholder="add note"
                >
                </textarea-autosize>
              </form>
            </v-card-text>

            <v-card-actions>
              <v-btn text color="secondary" @click="selectedOpen = false">
                Закрыть
              </v-btn>
              <v-btn
                v-if="currentlyEditing !== selectedEvent.id"
                text
                @click.prevent="editEvent(selectedEvent)"
              >
                Изменить
              </v-btn>
              <v-btn
                text
                v-else
                type="submit"
                @click.prevent="updateEvent(selectedEvent)"
              >
                Сохранить
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-menu>
      </v-sheet>
    </v-col>
  </v-row>
</template>

<script>
import Vue from "vue";
import db from "@/main";

export default Vue.component("graph", {
  // данные должны быть связаны с проектом
  data: () => ({
    today: new Date().toISOString().substr(0, 10),
    focus: new Date().toISOString().substr(0, 10),
    type: "week",
    typeToLabel: {
      month: "Месяц",
      week: "Неделя",
      "4day": "5/2",
    },
    names: [{ name: null }], //массив данных с сотрудниками по проекту
    start: null,
    end: null,
    color: "#1976D2", // цвет установленный по умолчанию
    currentlyEditing: null,
    selectedEvent: {},
    selectedElement: null,
    selectedOpen: false,
    events: [],
    dialog: false,
    dialogDate: false,
  }),
  mounted() {
    this.getEvents();

    const cal = this.$refs.calendar;
    // scroll to the current time
    const minutes = cal.times.now.hour * 60 + cal.times.now.minute;
    const firstTime = Math.max(0, minutes - (minutes % 30) - 30);
    cal.scrollToTime(firstTime);

    // every minute update the current time bar
    setInterval(() => cal.updateTimes(), 60 * 1000);
  },
  computed: {
    title() {
      const { start, end } = this;
      if (!start || !end) {
        return "";
      }
      const startMonth = this.monthFormatter(start);
      const startYear = start.year;
      const startDay = start.day;
      const endDay = end.day;
      switch (this.type) {
        case "month":
          return `${startYear}, \n${startMonth}`;
        case "week":
          return `${startYear}, \n${startMonth}: ${startDay} - ${endDay}`;
        case "day":
          return `${startYear}, \n${startMonth} ${startDay}`;
        case "4day":
          return `${startYear}, \n${startMonth}: ${startDay} - ${endDay}`;
      }
      return "";
    },
    monthFormatter() {
      return this.$refs.calendar.getFormatter({
        timeZone: `UTC`,
        month: "long",
      });
    },
  },

  methods: {
    async getEvents() {
      let snapshot = await db.collection("calEvent").get();
      const events = [];
      snapshot.forEach((doc) => {
        let appData = doc.data();
        appData.id = doc.id;
        events.push(appData);
      });
      this.events = events;
    },
    setDialogDate({ date }) {
      this.dialogDate = true;
      this.focus = date;
    },
    viewDay({ date }) {
      this.focus = date;
      this.type = "day";
    },
    getEventColor(event) {
      return event.color;
    },
    setToday() {
      this.focus = this.today;
    },
    prev() {
      this.$refs.calendar.prev();
    },
    next() {
      this.$refs.calendar.next();
    },
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
    }, //здесь метод для устанoвки окончания недель
  },
});
</script>

<style lang="scss" scoped>
.createEv {
  margin: 0.5em;
}

.curWeek {
  margin: 0.5em;
}
.v-calendar {
  user-select: none;
  -webkit-user-select: none;
}

.v-current-time {
  height: 1px;
  background-color: red;
  position: absolute;
  left: -1px;
  right: 0;
  pointer-events: none;

  &.first::after {
    content: "";
    position: absolute;
    background-color: red;
    width: 7px;
    height: 7px;
    border-radius: 3.5px;
    margin-top: -3px;
    margin-left: -3px;
  }

  .v-current-time-time {
    display: none;
  }

  &.first .v-current-time-time {
    display: block;
    position: absolute;
    left: -60px;
    color: red;
    font-size: 10px;
    padding: 1px;
    background-color: white;
    width: 50px;
    height: 20px;
    text-align: right;
    top: -9px;
  }
}

v-form v-btn {
  color: "white";
}
</style>
