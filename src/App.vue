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
import VHeader from '@/components/header'
import Entropy from '@/components/Entropy'
import Wallet from '@/components/Wallet'
export default {
  name: 'App',
  data () {
    return {
      page: Wallet,
      count: 0,
      entropy: null
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
