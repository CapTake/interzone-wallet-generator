<template>
  <div id="app">
    <v-header :name="coin" :url="site" />
    <div v-if="entropy">
      <wallet :pub="pub" :priv="priv" :name="coin" />
    </div>
    <entropy v-else @ready="entropyCollected" />
  </div>
</template>

<script>
// eslint-disable-next-line
import tachions from 'tachyons'
// import CryptoJS from 'cryptojs'
import bitcoin from 'bitcoinjs-lib'
import VHeader from '@/components/header'
import Entropy from '@/components/Entropy'
import Wallet from '@/components/Wallet'
export default {
  name: 'App',
  data () {
    return {
      page: Wallet,
      nonce: 0,
      entropy: null,
      coin: 'Interzone',
      site: 'https://interzone.scpace',
      pub: null,
      priv: null,
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
  components: {
    VHeader,
    Entropy,
    Wallet
  },
  methods: {
    entropyCollected (entropy) {
      let ecp = bitcoin.ECPair.makeRandom({network: this.network})
      this.pub = ecp.getAddress()
      this.priv = ecp.toWIF()
      this.entropy = entropy
      this.nonce++
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
