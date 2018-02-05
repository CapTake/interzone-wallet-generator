<template>
  <div id="app">
    <v-header :name="coin" :url="site" :sub="slogan" />
    <div v-if="entropy" class="mb5-ns">
      <div class="ph3 bg-black-90">
        <a class="f6 link dim ph3 pv2 mh1 dib br1 br--top" :class="buttonClass(0)" @click.prevent="pageClick(0, $event)" href="#0">Single address</a>
        <a class="f6 link dim ph3 pv2 mh1 dib br1 br--top" :class="buttonClass(1)" @click.prevent="pageClick(1, $event)" href="#1">Bulk wallet</a>
      </div>
      <div class="pv4 bb b--silver">
        <span class="b ma2 dib">Seed:</span>
        <label class="ma2 dib"><input type="radio" v-model="isRandom" :value="true" /> Random</label>
        <label class="ma2 dib"><input type="radio" v-model="isRandom" :value="false" /> Passphrase</label>
        <button class="f6 link dim br1 ph3 pv2 mb2 dib white bg-dark-blue b--none" :style="btnStyle" @click.prevent="onButton">Generate wallet</button>
        <div v-if="byPhrase" class="mw9 center mt3 ph3-ns">
          <div class="cf ph2-ns pb3">
            <div class="fl w-100 w-third-ns pb1">
              Password phrase
            </div>
            <div class="fl w-100 w-two-thirds-ns pb1">
              <input type="password" class="w-90 center ph2" v-model="pass" />
              <div v-if="pass.length === 0" class="w-90 center ph2 f7 mt1">
                Use long, impossible for anyone to guess passphrase, only you could remember
              </div>
              <div v-else-if="pass.length < minpasslen" class="w-90 center ph2 f7 mt1 red">
                Your password phrase is too short! (at least {{ minpasslen }} characters required)
              </div>
            </div>
          </div>
          <div class="cf ph2-ns pb3">
            <div class="fl w-100 w-third-ns pb1 right">
              Repeat pass phrase
            </div>
            <div class="fl w-100 w-two-thirds-ns pb1">
              <input type="password" class="w-90 center ph2" v-model="pass2" />
              <div v-if="pass2.length && pass2 !== pass" class="w-90 center ph2 f7 mt1 red">
                  Password phrases don't match
              </div>
            </div>
          </div>
        </div>
      </div>
      <wallet v-if="page === 0" :pub="one.pub" :priv="one.priv" :name="coin" :short="short" />
      <bulk-wallet v-if="page === 1" :wallets="bulk" :name="coin" :short="short" :working="waiting" />
    </div>
    <entropy v-else @ready="entropyCollected" :touch="touch" />
    <footer class="pv4 ph3 ph5-m ph6-l bg-silver black">
      <small class="f7 db tc"><b class="ttu">Show your support</b></small>
      <div class="center w-100 w-50-ns mv3 f7" style="word-break: break-all; word-wrap: break-word;">
      {{ coin + ': ' + support }}
      </div>
      <div class="tc mt3">
        <a href="https://github.com/CapTake/interzone-wallet-generator" title="Source code" class="f6 dib ph2 link mid-gray dim">Check source code</a>
      </div>
    </footer>
    <v-modal v-if="waiting" text="Working..." />
  </div>
</template>

<script>
// eslint-disable-next-line
import tachions from 'tachyons'
import { Buffer } from 'safe-buffer'
import bitcoin from 'bitcoinjs-lib'
import VHeader from '@/components/header'
import Entropy from '@/components/Entropy'
import VModal from '@/components/modal'
import Wallet from '@/components/Wallet'
import BulkWallet from '@/components/BulkWallet'
export default {
  name: 'App',
  data () {
    return {
      page: 0,
      minpasslen: 40,
      waiting: false,
      entropy: null,
      touch: false,
      coin: 'Interzone',
      short: 'ITZ',
      site: 'https://captake.github.io/interzone-wallet-generator/',
      support: '1MKVCJEsmeWHgeSUqtigBgLWp2Ncq1dd4p',
      slogan: 'Your Social Blockchain',
      one: {
        pub: null,
        priv: null
      },
      bulk: [],
      isRandom: true,
      pass: '',
      pass2: '',
      network: {
        messagePrefix: '\x19XXX Signed Message:\n',
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
    },
    btnStyle () {
      return (this.isRandom || this.passIsOK) && !this.waiting ? {} : {opacity: '0.5', cursor: 'not-allowed'}
    },
    passIsOK () {
      return this.pass.length >= this.minpasslen && this.pass === this.pass2
    }
  },
  components: {
    VHeader,
    Entropy,
    VModal,
    Wallet,
    BulkWallet
  },
  methods: {
    entropyCollected (entropy) {
      this.entropy = Buffer.from(entropy)
      this.one = this.genWallet(this.getRandom())
    },
    isLocal () {
      return window.location.protocol === 'file:'
    },
    getRandom () {
      let r = bitcoin.crypto.sha256(this.entropy)
      this.entropy = r
      return r
    },
    genWallets (seed, n) {
      return new Promise((resolve, reject) => {
        let wallets = []
        let nextseed = seed
        if (+n) {
          for (let i = 0; i < n; i++) {
            wallets.push(this.genWallet(nextseed))
            nextseed = bitcoin.crypto.sha256(nextseed)
          }
        }
        resolve(wallets)
      })
    },
    genWallet (seed) {
      let ecp = bitcoin.ECPair.makeRandom({
        network: this.network,
        rng: () => { return bitcoin.crypto.sha256(seed) }
      })
      return {pub: ecp.getAddress(), priv: ecp.toWIF()}
    },
    buttonClass (i) {
      return {
        white: i !== this.page,
        black: i === this.page,
        'bg-white': i === this.page,
        'bg-black': i !== this.page
      }
    },
    onButton (e) {
      e.target.blur()
      let seed = 0
      if (this.waiting) return
      if (this.isRandom) {
        seed = this.getRandom()
      } else {
        if (!this.passIsOK) return
        seed = Buffer.from(this.pass)
      }
      this.waiting = true
      setTimeout(() => {
        if (this.page) {
          this.genWallets(seed, 99).then((wallets) => {
            this.waiting = false
            this.bulk = wallets
          })
        } else {
          this.one = this.genWallet(seed)
          this.waiting = false
        }
      }, 100)
    },
    pageClick (i, e) {
      this.page = i
      e.target.blur()
    }
  },
  created () {
    let self = this
    window.addEventListener('touchstart', function touched () {
      self.touch = true
      window.removeEventListener('touchstart', touched)
    }, false)
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
