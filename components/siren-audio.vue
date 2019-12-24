<template>
  <div>
    <audio
      preload="auto"
      controls
    >
      <source src="tuesday-noon-siren.mp3" type="audio/mpeg">
      Your browser does not support the audio element.
    </audio>
    <div>
      {{ shouldPlayAudio }}
      {{ time }}
    </div>
    <button
      v-if="!isAudioEnabled"
      v-on:click="setAudioContext"
    >
      Load Audio
    </button>
    <button
      :disabled="!isAudioLoaded"
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
      audioBuffer: {},
      audioSource: {},
      audioContext: {},
      isAudioEnabled: false,
      isAudioLoaded: false,
      volume: 6
    }
  },
  watch: {
    shouldPlayAudio (oldVal, newVal) {
      console.log('ohha a change?', newVal)
      if (oldVal === false && newVal === true) {
        this.playAudio()
      }
    }
  },
  methods: {
    setAudioContext () {
      // TODO: see if we need more than just webkit handling here
      const audioContext = new (window.AudioContext || window.webkitAudioContext)()
      const gainNode = audioContext.createGain()

      this.audioContext = audioContext
      this.gainNode = gainNode
      this.getAudio()
      this.isAudioEnabled = true
    },
    getAudio () {
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
    decodeSuccess (buffer, audioPath) {
      console.log('ohhai a success?')
      this.audioBuffer = buffer
      this.loadAudioBuffer()
      this.isAudioLoaded = true
    },
    decodeError () {
      console.log('oh nutz an error?')
      this.isAudioEnabled = false
    },
    loadAudioBuffer () {
      const source = this.audioContext.createBufferSource()
      source.buffer = this.audioBuffer

      source.connect(this.audioContext.destination)
      source.connect(this.gainNode)
      this.gainNode.connect(this.audioContext.destination)
      this.setVolume(this.volume)
      this.audioSource = source
    },
    setVolume (volume) {
      this.gainNode.gain.value = volume
    },
    playAudio () {
      // this.audioSource.start()
      console.log('ohhai play audio', this.audioSource)
      this.audioSource.start()
    }
  }
}
</script>
