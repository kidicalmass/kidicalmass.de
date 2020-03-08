<template>
  <div class="city">
  <h1>{{ $page.frontmatter.city }}</h1>
  <div class="custom-block tip">
    <p class="custom-block-title">Nächster Termin</p>
    <p>In {{ timeTillNext }}</p>
  </div>
  <ul>
    <li v-for="date in dates">{{ date }}</li>
  </ul>
  </div>
</template>

<script>
import moment from 'moment-timezone'

export default {
  name: 'City',
  computed: {
    dates: function() {
      return this.$page.frontmatter.dates.map(date => moment.tz(date, 'Europe/Berlin').format('YYYY-MM-DD [um] HH:mm [Uhr]'))
    },
    now: function() { return moment().tz('Europe/Berlin') },
    timeTillNext: function() {
      var now = moment().tz('Europe/Berlin')
      console.log(moment.tz(this.$page.frontmatter.dates[0], 'Europe/Berlin').toISOString())
      console.log(moment.tz(this.$page.frontmatter.dates[0], 'Europe/Berlin').format('YYYY-MM-DD HH:mm'))
      var hours = Math.min( ...this.$page.frontmatter.dates.map(date => moment.tz(date, 'Europe/Berlin').diff(now, 'hours')).filter(date => date > 0) )
      if (hours > 24) {
        var time = Math.floor(hours / 24)
        var unit = time === 1 ? 'Tag' : 'Tagen'
      } else {
        var time = hours
        var unit = time === 1 ? 'Stunde' : 'Stunden'
      }
      return time + ' ' + unit
    }
  }
}
</script>
