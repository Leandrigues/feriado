<template>
  <div class="container">
    <div class="header">O próximo feriado nacional é...</div>
    <div class="holiday">{{ formattedDate }} - {{ formattedTitle }}</div>
    
  </div>
</template>

<script>

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
    }
  },
  computed: {
    formattedTitle() {
      return this.holiday.title
    },
    formattedDate() {
      return this.formattedDay + ' de ' + this.formattedMonth
    },
    formattedDay() {
      const day = this.holiday.date.split('-')[2]
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
      const month = this.holiday.date.split('-')[1]

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
