<template>
  <div class="container">
    <ApproveAudio />
    <div class="siren-wrapper">
      <Siren />
    </div>
    <div>
      <h1 class="title">
        sf-siren
      </h1>
      <h2 class="subtitle">
        Miss the Tuesday noon siren? Get your fix here!
      </h2>
      <!--
      <h2 class="subtitle">
        <SirenText />
      </h2>
      -->
    </div>
    <Clock
      v-bind:timezone-offset="480"
      v-bind:interval-time="500"
      @on-clock-time-change="handleClockTimeChange"
    >
      <template v-slot:default="clock">
        <div>
          {{ clock.dateTime }}
        </div>
      </template>
    </Clock>
    <SirenAudio
      v-bind:should-play-audio="isInTimeWindow"
      v-bind:time="dateTime"
    />
  </div>
</template>

<script>
import ApproveAudio from '~/components/approve-audio.vue'
import Siren from '~/components/siren.vue'
import SirenAudio from '~/components/siren-audio.vue'
// import SirenText from '~/components/siren-text.vue'
import Clock from '~/components/clock.vue'

export default {
  components: {
    ApproveAudio,
    Siren,
    SirenAudio,
    // SirenText
    Clock
  },
  data () {
    return {
      dateTime: new Date(),
      isInTimeWindow: undefined
    }
  },
  methods: {
    handleClockTimeChange ({ dateTime, isInTimeWindow }) {
      // only set value if it's changed
      if (this.isInTimeWindow !== isInTimeWindow) {
        this.isInTimeWindow = isInTimeWindow
        this.dateTime = dateTime
        // console.log('this.isInTimeWindow', this.isInTimeWindow)
        // console.log('this.dateTime', this.dateTime)
      }
    }
  }
}
</script>

<style scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.siren-wrapper {
  position: absolute;
  height: 100%;
  width: 100%;
  z-index: -1;
}

.siren-wrapper svg {
  fill: #CCC;
  opacity: 0.5;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
  word-break: break-word;
}
</style>
