<template>
  <div class="oi" fluid>
    <section
      class="calendarDesk"
      min-width="500px"
      v-if="!$vuetify.breakpoint.xsOnly"
    >
      <div class="month" :class="color">
        <v-icon x-large color="white" @click="changeMonth(false)"
          >mdi-chevron-left</v-icon
        >
        <div>
          <h2>{{ months[month] }}</h2>
          <h3>{{ new Date(date).getFullYear() }}</h3>
        </div>
        <v-icon x-large color="white" @click="changeMonth(true)"
          >mdi-chevron-right</v-icon
        >
      </div>
      <div class="weekDays">
        <div>Sun</div>
        <div>Mon</div>
        <div>Tue</div>
        <div>Wen</div>
        <div>Thu</div>
        <div>Fri</div>
        <div>Sat</div>
      </div>
      <div class="daysDesk justify-space-between">
        <div
          v-for="(day, ind) in divDays"
          @click="editEvent(day, day.Nday)"
          :class="day.class"
          class="pa-0 dayDesk"
          style="position: relative"
          :key="ind"
        >
          <div
            v-if="day.data"
            class="eventClassDesk"
            :class="day.data.color"
          ></div>
          {{ day.Nday }}
        </div>
      </div>
      <v-btn
        text
        class="red--text btnAdd"
        @click="(dialogEvent = true), (edit = {})"
        >Adicionar evento</v-btn
      >
    </section>
    <section v-else class="calendar">
      <div class="month" :class="color">
        <v-icon x-large color="white" @click="changeMonth(false)"
          >mdi-chevron-left</v-icon
        >
        <div>
          <h2>{{ months[month] }}</h2>
          <h3>{{ new Date(date).getFullYear() }}</h3>
        </div>
        <v-icon x-large color="white" @click="changeMonth(true)"
          >mdi-chevron-right</v-icon
        >
      </div>
      <div class="weekDays">
        <div>Sun</div>
        <div>Mon</div>
        <div>Tue</div>
        <div>Wen</div>
        <div>Thu</div>
        <div>Fri</div>
        <div>Sat</div>
      </div>
      <div class="days">
        <div
          v-for="(day, ind) in divDays"
          @click="editEvent(day, day.Nday)"
          :class="day.class"
          class="dayDivMob"
          style="position: relative"
          :key="ind"
        >
          <div v-if="day.data" class="eventClass" :class="day.data.color"></div>
          {{ day.Nday }}
        </div>
        <v-btn
          text
          class="red--text btnAdd"
          @click="(dialogEvent = true), (edit = {})"
          >Adicionar evento</v-btn
        >
      </div>
      <!-- <v-btn @click="teste()">Teste</v-btn> -->
    </section>
    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-title
          class="text-h5 lighten-2 text-center"
          :class="edit.color || 'grey'"
        >
          {{ new Date(edit.date).toDateString() }}
          <v-spacer></v-spacer>
          <v-btn v-if="!editing" icon @click="editing = true">
            <v-icon>mdi-pencil</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text class="mt-6 text-h6">
          <h3 class="text-center">Appointment</h3>
          <div class="d-flex align-center">
            <strong color="black--text" class="mr-2">Pacient:</strong>
            <span v-if="!editing"> {{ edit.pacient }}</span>
            <v-text-field v-else v-model="edit.pacient"></v-text-field>
          </div>
          <br />
          <strong color="black--text">Observation: </strong>
          <p v-if="!editing">
            {{ edit.observation || "Sem observações" }}
          </p>
          <div v-else class="pa-0">
            <v-textarea label="" v-model="edit.observation"></v-textarea>
          </div>
          <div class="col-7 pa-0 d-flex align-center" max-width="150px">
            <strong class="mr-2"> Color:</strong>
            <span v-if="!editing"> {{ edit.color }}</span>
            <v-select
              :items="items"
              v-if="editing"
              v-model="edit.color"
              class="text-h6"
            ></v-select>
          </div>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-btn v-if="!editing" @click="sheet = true" text color="red"
            >Delete</v-btn
          >
          <v-spacer></v-spacer>
          <v-btn v-if="!editing" color="primary" text @click="dialog = false">
            Close
          </v-btn>
          <v-btn v-else color="primary" text @click="saveEvent()"> Save </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogEvent" width="500">
      <v-card>
        <v-card-title
          class="text-h5 lighten-2 text-center"
          :class="edit.color || 'grey'"
        >
          <v-col cols="12" sm="6" md="4">
            <v-menu
              ref="menu"
              v-model="menu"
              :close-on-content-click="false"
              :return-value.sync="date"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template #activator="{ on, attrs }">
                <v-text-field
                  v-model="newDate"
                  label="Picker in menu"
                  prepend-icon="mdi-calendar"
                  readonly
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker v-model="newDate" no-title scrollable>
                <v-spacer></v-spacer>
                <v-btn text color="primary" @click="menu = false">
                  Cancel
                </v-btn>
                <v-btn text color="primary" @click="$refs.menu.save(newDate)">
                  OK
                </v-btn>
              </v-date-picker>
            </v-menu>
          </v-col>
          <v-spacer></v-spacer>
        </v-card-title>
        <v-card-text class="mt-6 text-h6">
          <h3 class="text-center">Appointment</h3>
          <div class="d-flex align-center">
            <strong color="black--text" class="mr-2">Pacient:</strong>
            <v-text-field v-model="edit.pacient"></v-text-field>
          </div>
          <br />
          <strong color="black--text">Observation: </strong>
          <div class="pa-0">
            <v-textarea label="" v-model="edit.observation"></v-textarea>
          </div>
          <div class="col-7 pa-0 d-flex align-center" max-width="150px">
            <strong class="mr-2"> Color:</strong>
            <v-select
              :items="items"
              v-model="edit.color"
              class="text-h6"
            ></v-select>
          </div>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-btn color="primary" text @click="dialogEvent = false">
            Close
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn @click="newEvent()" text color="green">Create</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-bottom-sheet
      v-model="sheet"
      :width="$vuetify.breakpoint.mdAndUp ? '20%' : '60%'"
    >
      <v-sheet class="text-center rounded-t-lg" height="150px">
        <div class="pt-5">Delete Event?</div>
        <v-btn class="mt-6 mr-2" color="error" @click="deleteEvent()"
          >Delete</v-btn
        >
        <v-btn class="mt-6 ml-2" color="indigo" dark @click="sheet = !sheet">
          Close
        </v-btn>
      </v-sheet>
    </v-bottom-sheet>
    <v-snackbar v-model="snackbar" :color="snackbarColor" :timeout="3000">
      {{ snackbarText }}
      <template #action="{ attrs }">
        <v-btn color="white" text v-bind="attrs" @click="snackbar = false">
          FECHAR
        </v-btn>
      </template>
    </v-snackbar>
  </div>
</template>

<script>
import api from "../plugins/axios.js";

export default {
  name: "HelloWorld",
  data() {
    return {
      newDate: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        .toISOString()
        .substr(0, 10),
      date: "",
      snackbar: false,
      snackbarColor: "",
      snackbarText: "",
      menu: false,
      dialogEvent: false,
      sheet: false,
      events: [],
      filteredEvents: [],
      editing: false,
      dialog: false,
      edit: {},
      items: [
        "blue-grey",
        "red",
        "pink",
        "purple",
        "indigo",
        "blue",
        "cyan",
        "teal",
        "green",
        "lime",
        "yellow",
        "orange",
        "brwon",
      ],
      month: "",
      color: "",
      divDays: [],
      lastDay: "",
      lastMLDay: "",
      months: [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ],
      dateString: "",
      date2: "",
    };
  },
  mounted() {
    this.date = new Date();
    this.getEvents();
    this.month = this.date.getMonth();
    this.dateString = this.date.toDateString();
  },
  watch: {
    month() {
      switch (this.month) {
        case 0: {
          this.color = "deep-purple";
          break;
        }
        case 1: {
          this.color = "purple";
          break;
        }
        case 2: {
          this.color = "blue-grey";
          break;
        }
        case 3: {
          this.color = "blue";
          break;
        }
        case 4: {
          this.color = "yellow";
          break;
        }
        case 5: {
          this.color = "red";
          break;
        }
        case 6: {
          this.color = "orange";
          break;
        }
        case 7: {
          this.color = "deep-purple";
          break;
        }
        case 8: {
          this.color = "teal";
          break;
        }
        case 9: {
          this.color = "pink";
          break;
        }
        case 10: {
          this.color = "cyan";
          break;
        }
        case 11: {
          this.color = "lime";
          break;
        }
      }
    },
  },
  methods: {
    teste() {
      console.log(new Date("June 21 2022"));
    },
    async newEvent() {
      const splited = this.newDate.split("-");
      this.edit.date =
        this.months[splited[1] - 1] + " " + splited[2] + " " + splited[0];
      console.log(this.edit);
      await api
        .post(`eventos`, this.edit)
        .then(() => {
          this.snackbar = true;
          this.snackbarText = "Event registered successfully!";
          this.snackbarColor = "green darken-3";
          this.edit = {};
          this.getEvents();
          this.dialogEvent = false;
        })
        .catch((res) => {
          this.snackbar = true;
          this.snackbarText =
            "Failed to register event, check if there is already an event for the requested date!";
          this.snackbarColor = "red darken-3";
          console.log(res.message);
        });
    },
    async deleteEvent() {
      await api
        .delete(`eventos/${this.edit._id}`)
        .then(() => {
          this.snackbar = true;
          this.snackbarText = "Event successfully deleted!";
          this.snackbarColor = "green darken-3";
          this.edit = {};
          this.getEvents();
          this.dialog = false;
          this.sheet = false;
        })
        .catch(() => {
          this.snackbar = true;
          this.snackbarText = "Failed to delete event!";
          this.snackbarColor = "red darken-3";
        });
    },
    async saveEvent() {
      if (this.edit._id) {
        await api
          .put(`eventos/${this.edit._id}`, this.edit)
          .then(() => {
            this.snackbar = true;
            this.snackbarText = "Event successfully edited!";
            this.snackbarColor = "green darken-3";
            this.getEvents();
            this.dialog = false;
            this.editing = false;
          })
          .catch(() => {
            this.snackbar = true;
            this.snackbarText = "Failed to edit event!";
            this.snackbarColor = "red darken-3";
          });
      } else {
        await api
          .post(`eventos`, this.edit)
          .then(() => {
            this.snackbar = true;
            this.snackbarText = "Event registered successfully";
            this.snackbarColor = "green darken-3";
            this.getEvents();
            this.dialog = false;
            this.editing = false;
          })
          .catch(() => {
            this.snackbar = true;
            this.snackbarText = "Failed to register event!";
            this.snackbarColor = "red darken-3";
          });
      }
    },
    editEvent(item, Nday) {
      if (item.month === "current") {
        if (item.data) {
          this.editing = false;
          this.edit = item.data;
        } else {
          const newDate = this.date;
          newDate.setDate(Nday);
          this.editing = true;
          this.edit = {
            color: "blue-grey",
            date: newDate,
          };
        }
        this.dialog = true;
      } else if (item.month === "prev") {
        this.changeMonth(false);
      } else {
        this.changeMonth(true);
      }
    },
    async getEvents() {
      this.date = new Date(this.date);
      await api.get("eventos").then((res) => {
        this.events = res.data;
        this.filterEvents();
      });
    },
    filterEvents() {
      const month = new Date(this.date).getMonth();
      const year = new Date(this.date).getFullYear();
      const ftEvents = [];
      for (let f = 0; f < this.events.length; f++) {
        if (
          new Date(this.events[f].date).getMonth() === month &&
          new Date(this.events[f].date).getFullYear() === year
        ) {
          ftEvents.push(this.events[f]);
        }
      }
      this.filteredEvents = ftEvents;
      this.loadCalendar();
    },
    changeMonth(bool) {
      if (bool) {
        this.date.setMonth(this.date.getMonth() + 1);
      } else {
        this.date.setMonth(this.date.getMonth() - 1);
      }
      this.dateString = this.date.toDateString();
      this.month = this.date.getMonth();
      this.getEvents();
    },
    loadCalendar() {
      this.date.setDate(1);
      this.divDays = [];
      this.lastDay = new Date(
        this.date.getFullYear(),
        this.date.getMonth() + 1,
        0
      ).getDay();
      const lastMonthDate = new Date(
        this.date.getFullYear(),
        this.date.getMonth() + 1,
        0
      ).getDate();
      this.lastMLDay = new Date(
        this.date.getFullYear(),
        this.date.getMonth(),
        0
      ).getDate();
      const dayIndex = this.date.getDay();
      const nextDays = 7 - this.lastDay - 1;
      for (let i = dayIndex; i > 0; i--) {
        const item = {
          Nday: this.lastMLDay - i + 1,
          class: "greyy dayDiv",
          month: "prev",
        };
        this.divDays.push(item);
      }
      for (let j = 1; lastMonthDate >= j; j++) {
        if (
          j === new Date().getDate() &&
          this.date.getMonth() === new Date().getMonth()
        ) {
          let item = { Nday: j, class: "today dayDiv", month: "current" };
          const obj = this.filteredEvents.filter((event) => {
            if (new Date(event.date).getDate() === j) {
              return event;
            }
          });
          if (obj) {
            item = {
              Nday: j,
              class: "today dayDiv",
              data: obj[0],
              month: "current",
            };
          } else {
            item = { Nday: j, class: "today dayDiv", month: "current" };
          }
          this.divDays.push(item);
        } else {
          let item = { Nday: j, class: "dayDiv" };
          const obj = this.filteredEvents.filter((event) => {
            if (new Date(event.date).getDate() === j) {
              return event;
            }
          });
          if (obj) {
            item = {
              Nday: j,
              class: "dayDiv",
              data: obj[0],
              month: "current",
            };
          } else {
            item = { Nday: j, class: "dayDiv", month: "current" };
          }
          this.divDays.push(item);
        }
      }
      for (let k = 1; k <= nextDays; k++) {
        const item = { Nday: k, class: "greyy dayDiv", month: "next" };
        this.divDays.push(item);
      }
    },
  },
};
</script>

<style scoped>
@media (min-width: 600px) and (max-width: 960px) {
  .calendarDesk {
    width: 600px;
  }
  .daysDesk {
    height: 500px;
  }
  .dayDesk {
    width: calc(550px / 7);
  }
}
@media (min-width: 960px) {
  .calendarDesk {
    width: 900px;
  }
  .daysDesk {
    height: 600px;
  }
  .dayDesk {
    width: calc(800px / 7);
  }
}
.calendarDesk {
  position: relative;
  align-items: center;
  justify-content: center;
  color: white;
  letter-spacing: 1px;
  text-align: center;
}
.dayDesk {
  overflow: hidden;
}
.calendar {
  position: relative;
  align-items: center;
  justify-content: center;
  width: 350px;
  color: white;
  letter-spacing: 1px;
  text-align: center;
}
.greyy {
  opacity: 0.5;
}
.btnAdd {
  position: absolute;
  bottom: 10px;
  font-weight: 700;
  right: 5%;
}

.month {
  overflow: hidden;
  position: relative;
  padding: 15px 10px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: green;
  text-shadow: 0 0.3rem 0.5rem rgba(0, 0, 0, 0.5);
}
.month div h2 {
  text-transform: uppercase;
  letter-spacing: 3px;
}
.weekDays {
  padding-bottom: 10px;
  display: flex;
  justify-content: space-around;
  font-size: 20px;
  background-color: rgb(40, 58, 58);
  text-shadow: 0 0.3rem 0.5rem rgba(0, 0, 0, 0.5);
}
.weekDays div {
  width: calc(350px / 8);
}
.days {
  display: flex;
  height: 370px;
  background-color: rgb(40, 58, 58);
  flex-wrap: wrap;
  text-shadow: 0 0.3rem 0.5rem rgba(0, 0, 0, 0.5);
  position: relative;
  padding-bottom: 30px;
}
.daysDesk {
  display: flex;
  background-color: rgb(40, 58, 58);
  flex-wrap: wrap;
  text-shadow: 0 0.3rem 0.5rem rgba(0, 0, 0, 0.5);
  position: relative;
  padding-bottom: 50px;
}
.dayDiv {
  cursor: pointer;
  transition: background-color 0.3s;
  border: 0.5px rgb(40, 58, 58) solid;
  margin: calc(7px / 2);
}
.dayDivMob {
  width: calc(301px / 7);
  height: calc(350px / 7);
}
.today {
  background-color: rgb(12, 146, 146);
}
.dayDiv:hover {
  background-color: rgb(28, 41, 41);
  border: 0.5px white solid;
}
.menu {
  position: absolute;
  top: 0px;
  z-index: 100;
}
.eventClass {
  position: absolute;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  top: 70%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.eventClassDesk {
  position: absolute;
  width: 100px;
  height: 100px;
  bottom: -60px;
  right: -60px;
  overflow: hidden;
  transform: rotate(-45deg);
}
</style>
