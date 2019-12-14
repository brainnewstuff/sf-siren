<script>
const INTERVAL_TIME = 200

export default {
  props: {
    timezoneOffset: {
      default: 0,
      type: Number
    }
  },
  data () {
    return {}
  },
  created () {
    this.setDateTime()
  },
  methods: {
    setDateTime () {
      const now = new Date()

      this.dateTime = new Date(
        now.getUTCFullYear(),
        now.getUTCMonth(),
        now.getUTCDate(),
        // TODO: get a daylight savings offset using the same trick I did at sqor
        now.getUTCHours(),
        now.getUTCMinutes() + this.timezoneOffset,
        now.getUTCSeconds()
      )
    },
    stopClock () {
      clearInterval(this.intervalId)
    },
    getIsNoon () {
    },
    startClock () {
      // TODO: interval that is smaller when we're super close to noon (within say 10 seconds?) and like half a second otherwise
      this.intervalId = setInterval(() => {
        this.getIsNoon()
        // maybe re-render?
      }, INTERVAL_TIME)
    }
  },
  render () {
    return this.$scopedSlots.default({
      dateTime: this.dateTime
    })
  }
}
</script>
