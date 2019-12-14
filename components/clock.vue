<script>
const INTERVAL_TIME = 500

export default {
  props: {
    timezoneOffset: {
      default: 0,
      type: Number
    }
  },
  data () {
    return {
      dateTime: ''
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

      const dateTime = new Date(
        now.getUTCFullYear(),
        now.getUTCMonth(),
        now.getUTCDate(),
        // TODO: get a daylight savings offset using the same trick I did at sqor
        now.getUTCHours(),
        now.getUTCMinutes() - this.timezoneOffset,
        now.getUTCSeconds()
      )

      this.$set(this, 'dateTime', dateTime)
    },
    stopClock () {
      clearInterval(this.intervalId)
    },
    // TODO; this is noon thing is prolly best handled at the page level or in a component that cares more about the siren start/stop times
    getIsNoon () {
    },
    startClock () {
      // TODO: interval that is smaller when we're super close to noon (within say 10 seconds?) and like half a second otherwise
      // NOTE: this won't work if I move getIsNoon outside...hmmmmmmmmmmmmmmmmmmmmmmm
      this.intervalId = setInterval(() => {
        this.setDateTime()
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
