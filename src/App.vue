<template>
  <div class="background" :style="{background: `linear-gradient(70deg, ${palettes[selectedColor].primary} 0%, ${palettes[selectedColor].secondary} 70%)`, color: palettes[selectedColor].text }">
    <div class="container">
      <div class="header">O próximo feriado nacional é...</div>
      <div class="holiday" v-if="nextHoliday">{{ formattedDate }} - {{ formattedTitle }}</div>
    </div>
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
      nextHoliday: undefined,
      palettes: [
        {
          primary:'#F79272',
          secondary: '#B589B4',
          text: '#FBFBFB'
        },
        {
          primary:'#8447FF',
          secondary: '#D972FF',
          text: '#FBFBFB'
        },
        {
          primary:'#a524fe',
          secondary: '#125d84',
          text: '#FBFBFB'
        },
        {
          primary:'#1df495',
          secondary: '#7405a6',
          text: '#FBFBFB'
        },
        {
          primary:'#64a1b9',
          secondary: '#f340dc',
          text: '#FBFBFB'
        },
        {
          primary:'#3fc9b9',
          secondary: '#b2f70c ',
          text: '#FBFBFB'
        },
      ],
      selectedColor: Math.ceil(((Math.random() * 100) % 5) - 1) 

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
</style>
