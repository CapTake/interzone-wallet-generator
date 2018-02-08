<template>
  <paper-wallet v-if="paper" :pub="pub" :priv="priv" :pubq="pubq" :privq="privq"  :logo="logo" :coin="name" :short="short" />
  <div v-else class="mw9 center mb4-ns bg-white">
  <div class="cf">
    <div class="fl w-100 w-50-ns pv4">
      <span class="b">Public address</span> <span class="bg-green pl1 pr1 white">OK to share</span>
      <div><img :src="pubq" class="mw-60 mv-80-ns mh-60 mh-80-ns center w5 h5" alt="Public address, you may share it" :key="pub" /></div>
      <div class="fw4 f7 ph2 code" style="word-break: break-all; word-wrap: break-word;">{{ pub }}</div>
    </div>
    <div class="fl w-100 w-50-ns pv4">
      <span class="b">Private key</span> <span class="bg-red pl1 pr1 white">KEEP SECRET!</span>
      <div><img :src="privq" class="mw-60 mv-80-ns  mh-60 mh-80-ns center w5 h5" alt="Private key, keep it safe! Anyone who has it controls your money!" :key="priv" /></div>
      <div class="fw4 f7 ph2 code" style="word-break: break-all; word-wrap: break-word;">{{ priv }}</div>
    </div>
  </div>
</div>
</template>

<script>
import QRCode from 'qrcode'
import PaperWallet from '@/components/PaperWallet'
export default {
  props: ['pub', 'priv', 'name', 'short', 'logo', 'paper'],
  data () {
    return {
      pubq: '',
      privq: ''
    }
  },
  components: {
    PaperWallet
  },
  methods: {
    onUpdate () {
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
  },
  created () {
    this.onUpdate()
  },
  updated () {
    this.onUpdate()
  }
}
</script>

<style>

</style>
