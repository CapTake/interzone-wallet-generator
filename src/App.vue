<template>
  <div id="app">
    <v-header />
    <component v-if="entropy" :is="page" />
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
      network: {
        messagePrefix: '\x19Litecoin Signed Message:\n',
        bip32: {
          public: 0x019da462,
          private: 0x019d9cfe
        },
        pubKeyHash: 0x3F,
        scriptHash: 0x99,
        wif: 0x99
      }
    }
  },
  components: {
    VHeader,
    Entropy
  },
  methods: {
    entropyCollected (entropy) {
      this.entropy = entropy
    }
  },
  mounted () {
    let ecp = bitcoin.ECPair.makeRandom({network: this.network})
    console.log(ecp.toWIF(), ecp.getAddress())
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
