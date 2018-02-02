<template>
  <div id="app">
    <v-header :name="coin" :url="site" :sub="slogan" />
    <div v-if="entropy" class="bb">
      <div class="ph3 bg-black-90">
        <a class="f6 link dim ph3 pv2 mh1 dib" :class="buttonClass(0)" @click.prevent="page = 0" href="#0">Single address</a>
        <a class="f6 link dim ph3 pv2 mh1 dib" :class="buttonClass(1)" @click.prevent="page = 1" href="#1">Many addresses</a>
      </div>
      <div class="pv4 bb">
        <span class="b">Seed:</span> <label class="mh2"><input type="radio" v-model="isRandom" value="true" /> Random</label> <label class="mh2"><input type="radio" v-model="isRandom" value="false" /> Passphrase</label>
        <div v-show="byPhrase" class="pa3 bt">
          <input type="password" />
          <input type="password" />
        </div>
      </div>
      <wallet :pub="pub" :priv="priv" :name="coin" />
    </div>
    <entropy v-else @ready="entropyCollected" />
  </div>
</template>

<script>
// eslint-disable-next-line
import tachions from 'tachyons'
import { Buffer } from 'safe-buffer'
import bitcoin from 'bitcoinjs-lib'
import VHeader from '@/components/header'
import Entropy from '@/components/Entropy'
import Wallet from '@/components/Wallet'
export default {
  name: 'App',
  data () {
    return {
      page: 0,
      nonce: 0,
      entropy: null,
      coin: 'Interzone',
      site: 'https://interzone.space',
      slogan: 'Catchy subheadline where we explain your wonderful new startup even more',
      pub: null,
      priv: null,
      bulk: [],
      isRandom: true,
      pass: '',
      pass2: '',
      network: {
        messagePrefix: '\x3fXXX Signed Message:\n',
        bip32: {
          public: 0x019da462,
          private: 0x019d9cfe
        },
        pubKeyHash: 0x00,
        scriptHash: 0x99,
        wif: 0x9c
      }
    }
  },
  computed: {
    byPhrase () {
      return !this.isRandom
    }
  },
  components: {
    VHeader,
    Entropy,
    Wallet
  },
  methods: {
    entropyCollected (entropy) {
      this.entropy = Buffer.from(entropy)
      let ecp = bitcoin.ECPair.makeRandom({
        network: this.network,
        rng: () => { return bitcoin.crypto.sha256(this.entropy) }
      })
      this.pub = ecp.getAddress()
      this.priv = ecp.toWIF()
      this.nonce++
    },
    buttonClass (i) {
      return {
        white: i !== this.page,
        black: i === this.page,
        'bg-white': i === this.page,
        'bg-black': i !== this.page
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
