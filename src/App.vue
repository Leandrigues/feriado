<template>
  <div class="background" :style="{
    background: `linear-gradient(
  135deg,
  ${palettes[selectedColor].primary} 0%,
  ${palettes[selectedColor].secondary} 100%
)`,
    color: palettes[selectedColor].text,
  }">
    <div class="container">
      <div class="header">
        O próximo feriado nacional é
        <strong class="week-day">{{ formattedWeekday }}</strong>...
      </div>
      <div class="holiday" v-if="nextHoliday">
        {{ formattedDate }} - {{ formattedTitle }}
      </div>
    </div>
  </div>
</template>

<script>
import json from "./data/holidays.json";

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
          primary: "#5E60CE",
          secondary: "#64DFDF",
          text: "#F8F9FA",
        },
        {
          primary: "#4C6EF5",
          secondary: "#A5D8FF",
          text: "#F8F9FA",
        },
        {
          primary: "#7C83FD",
          secondary: "#CDB4DB",
          text: "#FFFFFF",
        },
        {
          primary: "#4361EE",
          secondary: "#4895EF",
          text: "#F1F3F5",
        },
        {
          primary: "#3A5A40",
          secondary: "#A3B18A",
          text: "#F6F7EB",
        },
        {
          primary: "#6D597A",
          secondary: "#B56576",
          text: "#FAF9F9",
        },
        {
          primary: "#355070",
          secondary: "#6D597A",
          text: "#F1FAEE",
        },
        {
          primary: "#3F72AF",
          secondary: "#DBE2EF",
          text: "#112D4E",
        },
        {
          primary: "#588157",
          secondary: "#A3B18A",
          text: "#F1FAEE",
        },
        {
          primary: "#2B5876",
          secondary: "#4E4376",
          text: "#F8F9FA",
        }
      ],
      selectedColor: Math.ceil(((Math.random() * 100) % 10) - 1),
    };
  },
  beforeMount() {
    this.holidays.sort((a, b) => {
      const dateA = new Date(a.date);
      const dateB = new Date(b.date);
      return dateA - dateB;
    })
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
  font-family: 'Inter', sans-serif!important;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
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
