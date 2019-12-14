<template>
  <div>
    {{ textValue }}
  </div>
</template>

<script>
const WAH_TEXT = 'WAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA!!!!1'
/*
const THIS_IS_A_TEST = 'This is a test.'
const THIS_IS_A_TEST_OUTDOOR = 'This is a test of the out door warning system.'
const THIS_IS_JUST_A_TEST = 'This is only a test.'
*/
export default {
  props: {
    time: {
      default: '',
      type: String
    }
  },
  data () {
    return {
      textValue: 'ohhai!'
    }
  },
  watch: {
    time (newValue, oldValue) {
      this.changeText(newValue)
    }
  },
  methods: {
    changeText () {
      this.textValue = WAH_TEXT
    }
  }
}
</script>
