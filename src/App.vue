<template>
  <div class='wrapper'>
    <div class='container left'>
      <span class='header'>Your local time</span>
      <span class='time'>{{ localTime }}</span>
      <span class='date'>{{ localDate }}</span>
      <span class='timezone'>{{ localTimezone }}</span>
    </div>
    <div class='container right'>
      <span class='header'>Select an option</span>
      <span class='time'>{{ selectedTime }}</span>
      <span class='date'>{{ selectedDate }}</span>
      <select class='selection' v-model='selectedTimezone'>
        <option v-for='item in timezoneNames' :value='item'>{{ item }}</option>
      </select>
    </div>
  </div>
</template>

<script>
import moment from 'moment-timezone'

export default {
  name: 'app',
  data () {
    return {
      moment,
      localTime: moment().local().format('HH:mm:ss'),
      localDate: moment().local().format('DD MMM YYYY'),
      localTimezone: `${moment.tz.guess()}, ${moment().local().format('Z')}`,
      timezoneNames: moment.tz.names(),
      selectedTimezone: 'Africa/Abidjan',
      selectedTime: null,
      selectedDate: null
    }
  },
  created () {
    setInterval(() => {
      this.localTime = moment().local().format('HH:mm:ss')
      this.selectedTime = moment.tz(this.selectedTimezone).format('HH:mm:ss')
      this.selectedDate = moment.tz(this.selectedTimezone).format('DD MMM YYYY')
    }, 1000)
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Lato:300,400,700');

* {
  margin: 0;
  padding: 0;
}
.wrapper {
  display: flex;
  width: 100vw;
  height: 100vh;
  font-family: 'Lato'
}
.container {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.container.left {
  background: linear-gradient(to right, #c02425, #f0cb35);
}
.container.right {
  background: linear-gradient(to right, #403a3e, #be5869);
}
.header {
  color: #eee;
  font-size: 1.5em;
  text-transform: uppercase;
  margin-bottom: 2em;
}
.time {
  font-size: 3em;
  color: #fff;
}
.date, .timezone {
  color: #eee;
}
.selection, .timezone {
  margin-top: 10px;
}
</style>
