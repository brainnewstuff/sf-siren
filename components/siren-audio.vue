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
    <button
      v-if="!isAudioEnabled"
      v-on:click="initializeAudio"
    >
      Initialize Audio
    </button>
    <button
      v-if="isAudioEnabled"
      v-on:click="playAudio"
    >
      Play
    </button>
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
      context: {},
      isAudioEnabled: false
    }
  },
  beforeMount () {
    // TODO: see if we need more than just webkit handling here
    const audioContext = new (window.AudioContext || window.webkitAudioContext)()
    // const gainNode = audioContext.createGain()

    this.audioContext = audioContext
  },
  mounted () {
  },
  methods: {
    initializeAudio () {
      console.log('ohhai a click?', this.audioContext)
      this.loadAudio()
    },
    loadAudio () {
      const request = new XMLHttpRequest()
      const audioPath = 'tuesday-noon-siren.mp3'

      request.open('GET', audioPath, true)
      request.responseType = 'arraybuffer'
      request.onload = () => {
        const { response } = request
        this.audioContext.decodeAudioData(
          response,
          (buffer) => {
            this.decodeSuccess(buffer, audioPath)
          },
          this.decodeError
        )
      }
      request.send()
    },
    decodeSuccess () {
      console.log('ohhai a success?')
      this.isAudioEnabled = true
    },
    decodeError () {
      console.log('oh nutz an error?')
    },
    playAudio () {
      console.log('ohhai play audio')
    }
  }
}
</script>
