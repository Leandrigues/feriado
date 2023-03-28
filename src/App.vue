<template>
  <div class="container">
    <div class="header">O próximo feriado nacional é...</div>
    <div class="holiday" v-if="nextHoliday">{{ formattedDate }} - {{ formattedTitle }}</div>
  </div>
</template>

<script>
import axios from 'axios';


export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      holiday: {
        date: '2022-01-01',
        title: 'Corpus Christi'
      },
      token: '3058|bfBc3asxUIB7IuWjgwU5FtHeNJndxfLP',
      base_url: 'https://api.invertexto.com/v1/holidays/2023?token=',
      nextHoliday: undefined
    }
  }, 
  beforeMount() {
    axios.get(`${this.base_url}${this.token}`).then((response) => {
      const holidaysList = response.data
      const current_date = new Date()

      this.nextHoliday = holidaysList.find((holiday) => {
        const date = new Date(holiday.date)
        return date >= current_date
      })
   })
  },
  computed: {
    formattedTitle() {
      return this.nextHoliday.name
    },
    formattedDate() {
      return this.formattedDay + ' de ' + this.formattedMonth
    },
    formattedDay() {
      console.log('Computed')
      const day = this.nextHoliday.date.split('-')[2]
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
        "Dezembro"
      ]
      const month = this.nextHoliday.date.split('-')[1]

      return months[parseInt(month) - 1]
    }
  }
}

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

body, html {
  margin: 0;
  padding: 0;
  height: 100%;
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
</style>
