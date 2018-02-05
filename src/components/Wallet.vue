<template>
  <div class="mw9 center ph3-ns bg-white">
  <div class="cf ph2-ns">
    <div class="fl w-100 w-50-ns pt4 pb4">
      <span class="b">Public address</span> <span class="bg-green pl1 pr1 white">OK to share</span>
      <div><img :src="pubq" class="mw5 w5 h5" alt="Public address, you may share it" /></div>
      <div class="h3 fw6 f7 pr2 pl2">{{ pub }}</div>
    </div>
    <div class="fl w-100 w-50-ns pt4 pb4">
      <span class="b">Private key</span> <span class="bg-red pl1 pr1 white">KEEP SECRET!</span>
      <div><img :src="privq" class="mw5 w5 h5" alt="Private key, keep it safe! Anyone who has it controls your money!" /></div>
      <div class="h3 fw6 f7 pr2 pl2" style="word-break: break-all; word-wrap: break-word;">{{ priv }}</div>
    </div>
  </div>
</div>
</template>

<script>
import QRCode from 'qrcode'
export default {
  props: ['pub', 'priv', 'name', 'short'],
  data () {
    return {
      pubq: '',
      privq: '',
      step: 0
    }
  },
  created () {
    QRCode.toDataURL(this.pub).then(url => {
      this.pubq = url
    }).catch(err => {
      console.error(err.message)
    })
    QRCode.toDataURL(this.priv).then(url => {
      this.privq = url
    }).catch(err => {
      console.error(err.message)
    })
  }
}
</script>

<style>

</style>
