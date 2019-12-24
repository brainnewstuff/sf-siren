<script>
export default {
  props: {
    timezoneOffset: {
      default: 0,
      type: Number
    },
    intervalTime: {
      default: 500,
      type: Number
    }
  },
  data () {
    return {
      dateTime: {},
      isInTimeWindow: false
    }
  },
  watch: {
    intervalTime (val, oldVal) {
      // if the interval changes, clear the
      // current one and start a new one
      if (val !== oldVal) {
        this.stopClock()
        this.startClock()
      }
    }
  },
  created () {
    this.setDateTime()
    this.startClock()
  },
  beforeDestroy () {
    this.stopClock()
  },
  methods: {
    setDateTime () {
      const now = new Date()

      let dateTime = new Date(
        now.getUTCFullYear(),
        now.getUTCMonth(),
        now.getUTCDate(),
        now.getUTCHours(),
        now.getUTCMinutes() - this.timezoneOffset,
        now.getUTCSeconds()
      )

      dateTime = this.handleDST(dateTime)

      this.$set(
        this,
        'dateTime',
        dateTime
      )
    },
    stopClock () {
      clearInterval(this.intervalId)
    },
    // TODO: this is noon thing is prolly best handled at the page level or in a component that cares more about the siren start/stop times
    // TODO: hrm or maybe just expose a boolean to the outside world... 🤔
    getIsInTimeWindow () {
      const hours = this.dateTime.getHours()
      const minutes = this.dateTime.getMinutes()
      const seconds = this.dateTime.getSeconds()
      // TODO: figure out a way to make this variable using props or by moving logic out of this component
      // TODO: also make sure we only do this on a tuesday
      const isInTimeWindow = hours === 13 && minutes === 19 && seconds <= 30

      return isInTimeWindow
    },
    handleTimeChange () {
      const isInTimeWindow = this.getIsInTimeWindow()
      const payload = {
        dateTime: this.dateTime,
        isInTimeWindow
      }

      this.setDateTime()
      this.$emit('on-clock-time-change', payload)
    },
    startClock () {
      // TODO: interval that is smaller when we're super close to noon (within say 10 seconds?) and like half a second otherwise
      // NOTE: this won't work if I move getIsNoon outside...hmmmmmmmmmmmmmmmmmmmmmmm
      this.intervalId = setInterval(this.handleTimeChange, this.intervalTime)
    },
    // handle daylight savings time
    // lovingly lifted from http://javascript.about.com/library/bldst.htm
    stdTimezoneOffset (dateTime) {
      const jan = new Date(dateTime.getFullYear(), 0, 1)
      const jul = new Date(dateTime.getFullYear(), 6, 1)

      return Math.max(jan.getTimezoneOffset(), jul.getTimezoneOffset())
    },
    dst (dateTime) {
      return dateTime.getTimezoneOffset() < this.stdTimezoneOffset(dateTime)
    },
    addHours (dateTime, hours) {
      dateTime.setTime(dateTime.getTime() + (hours * 60 * 60 * 1000))
      return dateTime
    },
    subtractHours (dateTime, hours) {
      dateTime.setTime(dateTime.getTime() - (hours * 60 * 60 * 1000))
      return dateTime
    },
    handleDST (dateTime) {
      // convert timezone offset to hours, and subtract those hours from date instantiated from an ISO timestamp
      dateTime = this.subtractHours(dateTime, dateTime.getTimezoneOffset() / 60)
      // if we detect that it is not daylight savings, add an hour to the time
      if (!this.dst(dateTime)) {
        dateTime = this.addHours(dateTime, 1)
      }

      return dateTime
    }
  },
  render () {
    return this.$scopedSlots.default({
      dateTime: this.dateTime
    })
  }
}
</script>
