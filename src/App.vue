<template>
  <div class='wrapper'>
    <div class='container left'>
      <span class='header'>Your local time</span>
      <span class='time'>{{ localTime }}</span>
      <span class='date'>{{ localDate }}</span>
      <span class='timezone'>{{ localTimezone }}</span>
    </div>
    <div class='container right'>
      <div class='content' ref='content'>
        <span class='header'>Select an option</span>
        <span class='time'>{{ selectedTime }}</span>
        <span class='date'>{{ selectedDate }}</span>
      </div>
      <div class='dropdown' tabindex="0" @blur='dismissDropdown()'>
        <div class='selected' @click='toggleDropdown()'>
          <span class='text'>{{ selectedTimezone }}</span>
          <span class='icon'>&#x25B2;</span>
        </div>
        <input class='search' v-show='dropdownExpanded' v-model='query' type="text" placeholder="Search..." @focus='interceptBlur()'>
        <div v-show='dropdownExpanded' class='selection'>
          <div class='item' v-for='item in filteredList' @click='selectItem(item)'>{{ item }}</div>
        </div>
      </div>
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
      selectedDate: null,
      dropdownExpanded: false,
      query: ''
    }
  },
  created () {
    setInterval(() => {
      this.localTime = moment().local().format('HH:mm:ss')
      this.selectedTime = moment.tz(this.selectedTimezone).format('HH:mm:ss')
      this.selectedDate = moment.tz(this.selectedTimezone).format('DD MMM YYYY')
    }, 1000)

    this.timezoneNames = this.timezoneNames.map(tz => {
      if (tz.indexOf('/') > -1) {
        tz = tz.split('/')
        tz = tz.splice(tz.length - 1, 1)
        tz = tz.join('')
      }
      tz = tz.replace('_', ' ')
      return tz
    })
  },
  methods: {
    toggleDropdown () {
      this.dropdownExpanded = true
      this.$refs.content.style.filter = 'blur(5px)'
    },
    selectItem (item) {
      this.selectedTimezone = item
      this.dismissDropdown()
      this.query = ''
    },
    dismissDropdown () {
      this.dropdownExpanded = false
      this.$refs.content.style.filter = 'blur(0)'
    }
  },
  computed: {
    filteredList () {
      return this.timezoneNames.filter(name => name.toLowerCase().indexOf(this.query.toLowerCase()) > -1)
    }
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
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
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
.dropdown, .timezone {
  margin-top: 10px;
}
.dropdown {
  margin-top: 10px;
  color: #eee;
  border-bottom: 1px solid #eee;
  padding: 10px;
  cursor: pointer;
  position: relative;
  outline: none;
  min-width: 170px;
  max-width: 170px;
}
.selected {
  display: flex;
  align-items: center;
  letter-spacing: 2px;
  justify-content: center;
  width: 100%;
}
.text {
  width: 100%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  text-align: center;
}
.icon {
  font-size: 0.5em;
  color: #ddd;
  margin-left: 10px;
}
.search {
  top: -345px;
  position: absolute;
  width: 246px;
  left: -30px;
  background: none;
  border: none;
  outline: none;
  background: rgba(255,255,255,0.6);
  padding: 10px;
  box-sizing: border-box;
  color: #eee;
  border-top: 3px solid #eee;
  border-right: 3px solid #eee;
  border-left: 3px solid #eee;
}
.search::placeholder {
  color: #fff;
}
.selection {
  width: 240px;
  height: 300px;
  overflow-y: scroll;
  background-color: rgba(255,255,255,0.6);
  border-right: 3px solid #eee;
  border-bottom: 3px solid #eee;
  border-left: 3px solid #eee;
  position: absolute;
  top: -310px;
  left: -30px;
}
.selection::-webkit-scrollbar {
  width: 7px;
}
.selection::-webkit-scrollbar-track {
  background: transparent;
}
.selection::-webkit-scrollbar-thumb {
  background-color: #ddd;
  border-radius: 10px;
}
.item {
  padding: 10px;
}
.item:hover {
  background-color: rgba(255,255,255,0.3)
}
</style>
