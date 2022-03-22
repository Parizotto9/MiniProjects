<template>
  <div class="calendar">
    <div class="month">
      <v-icon x-large color="white" @click="changeMonth(false)"
        >mdi-chevron-left</v-icon
      >
      <div>
        <h2>{{ months[month] }}</h2>
        <h4>{{ dateString }}</h4>
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
      <div v-for="(day, ind) in divDays" :class="day.class" :key="ind">
        {{ day.Nday }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      date: "",
      month: "",
      divDays: [],
      lastDay: "",
      lastMLDay: "",
      months: [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Set",
        "Oct",
        "Nov",
        "Dec",
      ],
      dateString: "",
      date2: "",
    };
  },
  mounted() {
    this.date = new Date();
    this.loadCalendar();
    this.month = this.date.getMonth();
    (this.dateString = this.date.toDateString()),
      console.log(
        this.date.toDateString(),
        this.date.getDay(),
        this.date.getDate(),
        this.date.getMonth(),
        this.date.getFullYear(),
        new Date(this.date.getFullYear(), this.date.getMonth() + 1, 0).getDate()
      );
  },
  methods: {
    changeMonth(bool) {
      if (bool) {
        this.date.setMonth(this.date.getMonth() + 1);
      } else {
        this.date.setMonth(this.date.getMonth() - 1);
      }
      this.dateString = this.date.toDateString();
      this.month = this.date.getMonth();
      this.loadCalendar();
    },
    loadCalendar() {
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
      console.log(this.date.getDay(), this.lastMLDay, "alo");
      for (let i = dayIndex; i >= 0; i--) {
        const item = { Nday: this.lastMLDay - i, class: "greyy dayDiv" };
        this.divDays.push(item);
        console.log(this.lastMLDay - i, "oi");
      }
      console.log(lastMonthDate, "lastJ");
      for (let j = 1; lastMonthDate >= j; j++) {
        console.log(j, "j");
        if (
          j === new Date().getDate() &&
          this.date.getMonth() === new Date().getMonth()
        ) {
          const item = { Nday: j, class: "today dayDiv" };
          this.divDays.push(item);
        } else {
          const item = { Nday: j, class: "dayDiv" };
          this.divDays.push(item);
        }
      }
      console.log(
        new Date(this.date.getFullYear(), this.date.getMonth() + 1, 0).getDay()
      );
      console.log(nextDays, this.lastMLDay, "nex");
      for (let k = 1; k <= nextDays; k++) {
        const item = { Nday: k, class: "greyy dayDiv" };
        this.divDays.push(item);
      }
      console.log(dayIndex);
      this.$forceUpdate();
    },
  },
};
</script>

<style scoped>
.calendar {
  align-items: center;
  justify-content: center;
  width: 350px;
  color: white;
  letter-spacing: 1px;
  text-align: center;
}
.greyy {
  opacity: 1/2;
  color: aquamarine;
}

.month {
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
  height: 350px;
  background-color: rgb(40, 58, 58);
  flex-wrap: wrap;
  text-shadow: 0 0.3rem 0.5rem rgba(0, 0, 0, 0.5);
}
.dayDiv {
  margin: calc(7px / 2);
  width: calc(301px / 7);
  height: calc(350px / 7);
  cursor: pointer;
  transition: background-color 0.3s;
  border: 0.5px rgb(40, 58, 58) solid;
}
.dayDiv:hover {
  background-color: rgb(28, 41, 41);
  border: 0.5px white solid;
}
</style>
