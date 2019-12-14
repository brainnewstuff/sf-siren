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
        <SirenText />
      </h2>
    </div>
    <Timer />
    <SirenAudio />
  </div>
</template>

<script>
import ApproveAudio from '~/components/approve-audio.vue'
import Siren from '~/components/siren.vue'
import SirenAudio from '~/components/siren-audio.vue'
import SirenText from '~/components/siren-text.vue'
import Timer from '~/components/timer.vue'

export default {
  components: {
    ApproveAudio,
    Siren,
    SirenAudio,
    SirenText,
    Timer
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
  word-break: break-all;
}
</style>
