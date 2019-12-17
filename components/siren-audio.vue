<template>
  <div>
    <audio
      preload="auto"
      controls
    >
      <source src="tuesday-noon-siren.mp3" type="audio/mpeg">
      Your browser does not support the audio element.
    </audio>
    <!--
    <div>
      {{ shouldPlayAudio }}
      {{ time }}
    </div>
    -->
  </div>
</template>

<script>
export default {
  props: {
    shouldPlayAudio: {
      default: false,
      type: Boolean
    },
    time: {
      default: new Date(),
      type: Date
    }
  },
  data () {
    return {
      context: {}
    }
  },
  mounted () {
    // do this in a browser only lifecycle method
    const AudioContext = window.AudioContext || window.webkitAudioContext
    const context = new AudioContext()

    this.context = context
  },
  methods: {
    doThing () {
    }
  }
}
</script>
