<script>
import dateTime from '~/mixins/date-time'

export default {
  mixins: [dateTime],
  props: {
    timezone: {
      default: 'UTC',
      type: String
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
      const dateTime = new Date()
      // this is a modern method which should be good enough for always rendering one
      // specific time zone regardless of where the user is viewing this page from
      dateTime.toLocaleString({ timeZone: this.timeZone })

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
        // TODO: shitty order dependence
        // `this.setDateTime()` sets datetime to `this`
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
    }
  },
  render () {
    return this.$scopedSlots.default({
      dateTime: this.dateTime
    })
  }
}
</script>
