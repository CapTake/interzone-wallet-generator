<template>
<header class="sans-serif">
  <div class="cover bg-left bg-center-l" style="background: #222">
    <div class="bg-black-80 pb5 pb6-m pb7-l">
      <nav class="dt w-100 mw8 center">
        <div class="dtc w2 v-mid pa3">
          <a href="/" class="dib w2 h2 pa1 ba b--white-90 grow-large border-box">
            <img class="link br1 h2 w2 dib" :src="logo" >
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
        <h1 class="f2 f1-l fw2 white-90 mb0 lh-title">This is your super impressive headline</h1>
        <h2 class="fw1 f3 white-80 mt3 mb4">Now a subheadline where explain your wonderful new startup even more</h2>
        <a class="f6 no-underline grow dib v-mid bg-blue white ba b--blue ph3 pv2 mb3" href="/">Call to Action</a>
        <span class="dib v-mid ph3 white-70 mb3">or</span>
        <a class="f6 no-underline grow dib v-mid white ba b--white ph3 pv2 mb3" href="">Secondary call to action</a>
      </div>
    </div>
  </div>
</header>
</template>

<script>
import logo from '../assets/logo.png'
export default {
  logo
}
</script>

<style>

</style>
