<template>
  <div
    class="background"
    :style="{
      background: `linear-gradient(70deg, ${palettes[selectedColor].primary} 0%, ${palettes[selectedColor].secondary} 70%)`,
      color: palettes[selectedColor].text,
    }"
  >
    <div class="container">
      <div class="header">
        O próximo feriado nacional é
        <strong class="week-day">{{ formattedWeekday }}</strong
        >...
      </div>
      <div class="holiday" v-if="nextHoliday">
        {{ formattedDate }} - {{ formattedTitle }}
      </div>
    </div>
  </div>
</template>

<script>
import json from "./data/holidays.json";
import axios from "axios";

export default {
  name: "App",
  components: {},
  data() {
    return {
      holiday: {
        date: "",
        title: "",
      },
      holidays: json,
      nextHoliday: undefined,
      palettes: [
        {
          primary: "#8447FF",
          secondary: "#D972FF",
          text: "#FBFBFB",
        },
        {
          primary: "#1df495",
          secondary: "#7405a6",
          text: "#FBFBFB",
        },
        {
          primary: "#64a1b9",
          secondary: "#f340dc",
          text: "#FBFBFB",
        },
      ],
      selectedColor: Math.ceil(((Math.random() * 100) % 3) - 1),
    };
  },
  beforeMount() {
    const holidaysList = this.holidays;
    const current_date = new Date();

    this.nextHoliday = holidaysList.find((holiday) => {
      const date = new Date(holiday.date);
      return date >= current_date;
    });
    this.weekday = new Date(this.nextHoliday.date).getUTCDay();
  },
  computed: {
    formattedTitle() {
      return this.nextHoliday.name;
    },
    formattedDate() {
      return this.formattedDay + " de " + this.formattedMonth;
    },
    formattedDay() {
      const day = this.nextHoliday.date.split("-")[2];
      return day;
    },
    formattedMonth() {
      const months = [
        "Janeiro",
        "Fevereiro",
        "Março",
        "Abril",
        "Maio",
        "Junho",
        "Julho",
        "Agosto",
        "Setembro",
        "Outubro",
        "Novembro",
        "Dezembro",
      ];
      const month = this.nextHoliday.date.split("-")[1];

      return months[parseInt(month) - 1];
    },
    formattedWeekday() {
      const weekdays = [
        "domingo",
        "segunda-feira",
        "terça-feira",
        "quarta-feira",
        "quinta-feira",
        "sexta-feira",
        "sábado",
      ];

      return weekdays[this.weekday];
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  align-items: center;
  height: 100%;
  justify-content: center;
}

body,
html {
  margin: 0;
  padding: 0;
  height: 100%;
}

.background {
  height: 100%;
  width: 100%;
  display: flex;
  align-items: center;
  height: 100%;
  justify-content: center;
}

.container {
  display: flex;
  flex-direction: column;
  text-align: left;
}

.header {
  font-size: 1.3rem;
}

.holiday {
  font-size: 4rem;
}

@media (max-width: 800px) {
  .container {
    text-align: center;
    justify-content: center;
    font-size: 0.1rem !important;
  }

  .header {
    font-size: 1rem;
  }

  .holiday {
    font-size: 2.5rem;
  }
}
</style>
