<template>
<header class="sans-serif">
  <div class="cover bg-left bg-center-l">
    <div class="bg-black-90 pb3">
      <nav class="dt w-100 mw8 center">
        <div class="dtc w2 v-mid pa3">
          <a :href="url" class="dib w2 h2 grow-large border-box">
            <img class="link h2 w2 dib" :src="logo" >
          </a>
        </div>
        <div class="dtc v-mid tr pa3">
          <a class="f6 fw4 hover-white no-underline white-70 dn dib-ns pv2 ph3" href="/" >How it Works</a>
          <a class="f6 fw4 hover-white no-underline white-70 dn dib-ns pv2 ph3" href="/" >Pricing</a>
          <a class="f6 fw4 hover-white no-underline white-70 dn dib-l pv2 ph3" href="/" >About</a>
          <a class="f6 fw4 hover-white no-underline white-70 dn dib-l pv2 ph3" href="/" >Careers</a>
          <a class="f6 fw4 hover-white no-underline white-70 dib ml2 pv2 ph3 ba" href="/" >Sign Up</a>
        </div>
      </nav>
      <div class="tc-l mt4 mt5-m mt6-l ph3">
        <h1 class="f2 f1-l fw2 white-90 mb0 lh-title">{{ name }} wallet generator</h1>
        <h2 class="fw1 f3 white-80 mt3 mb4">{{ sub }}</h2>
      </div>
    </div>
  </div>
</header>
</template>

<script>
import logo from '../assets/logo.png'
export default {
  props: ['name', 'url', 'sub'],
  data () {
    return {
      logo
    }
  }
}
</script>

<style>

</style>
