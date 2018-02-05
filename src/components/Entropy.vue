<template>
<div class="flex flex-column pv6 mb6-ns">
  <div class="w-100 pa3">
    Gathering entropy: {{ text }}
  </div>
  <div class="w-100 pv3">
    <div class="w-90 w-60-ns center">
      <v-progress :percent="percent" />
    </div>
  </div>
  <div class="w-100 pv3 ph1 ph3-ns f5" style="word-break: break-all; word-wrap: break-word;">
    {{ hex }}
  </div>
</div>
</template>

<script>
import {Buffer} from 'safe-buffer'
import bitcoin from 'bitcoinjs-lib'
import VProgress from '@/components/progress'
export default {
  props: ['touch'],
  data () {
    return {
      percent: 0,
      entropy_: ''
    }
  },
  computed: {
    entropy: {
      set (buffer) {
        let new_ = Buffer.from(buffer).toString() + this.entropy_

        this.entropy_ = bitcoin.crypto.sha256(Buffer.from(new_))
      },
      get () {
        return this.entropy_
      }
    },
    hex () {
      return this.entropy_.toString('hex')
    },
    text () {
      return 'Randomly ' + (this.touch ? 'tap the screen or ' : '') + 'move mouse pointer'
    }
  },
  components: {
    VProgress,
    bitcoin
  },
  mounted () {
    this.entropy = new Buffer(Array.from(window.crypto.getRandomValues(new Uint8Array(32))))
    let toGo = 200
    let f = (e) => {
      if (e.type === 'touchmove') {
        this.entropy = new Buffer([e.touches[0].screenX % 4, e.touches[0].screenY % 4])
      } else {
        this.entropy = new Buffer([e.screenX % 4, e.screenY % 4])
      }
      toGo = Math.max(toGo - 1, 0)
      this.percent = (200 - toGo) / 2
      if (toGo === 0) {
        document.body.removeEventListener('mousemove', f)
        document.body.removeEventListener('touchstart', f)
        this.$emit('ready', this.entropy)
      }
    }
    document.body.addEventListener('mousemove', f)
    document.body.addEventListener('touchstart', f)
  }
}
</script>

<style>

</style>
