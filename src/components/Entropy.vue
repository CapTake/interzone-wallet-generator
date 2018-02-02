<template>
<div class="flex flex-column">
  <div class="w-100 pa3">
    Gathering entropy
  </div>
  <div class="outline w-100 pa3">
    <v-progress :percent="percent" />
  </div>
  <div class="w-100 pa3 f7" style="word-break: break-all;    word-wrap: break-word;">
    {{ entropy }}
  </div>
</div>
</template>

<script>
// import CryptoJS from 'cryptojs'
import VProgress from '@/components/progress'
export default {
  data () {
    return {
      percent: 0,
      entropy_: ''
    }
  },
  computed: {
    entropy: {
      set (v) {
        let hex = v.map(b => ('00' + b.toString(16)).slice(-2)).join('')
        this.entropy_ = this.entropy_ + hex
      },
      get () {
        return this.entropy_
      }
    }
  },
  components: {
    VProgress
  },
  mounted () {
    this.entropy = Array.from(window.crypto.getRandomValues(new Uint8Array(64)))
    let toGo = 1000
    let f = (e) => {
      if (e.type === 'touchmove') {
        this.entropy = [e.touches[0].screenX % 4, e.touches[0].screenY % 4]
      } else {
        this.entropy = [e.screenX % 4, e.screenY % 4]
      }
      toGo = Math.max(toGo - 1, 0)
      this.percent = (1000 - toGo) / 10
      if (toGo === 0) {
        document.body.removeEventListener('mousemove', f)
        document.body.removeEventListener('touchmove', f)
        this.$emit('ready', this.entropy)
      }
    }
    document.body.addEventListener('mousemove', f)
    document.body.addEventListener('touchmove', f)
  }
}
</script>

<style>

</style>
