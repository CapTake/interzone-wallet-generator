<template>
  <div class="fixed absolute--fill flex items-center" style="background: rgba(0,0,0, .85); z-index: 10000">
    <div class="bg-white black br1 center pa3">
      {{ text }}
    </div>
  </div>
</template>

<script>
export default {
  props: ['text'],
  mounted () {
    document.body.style.overflow = 'hidden'
  },
  beforeDestroy () {
    document.body.style.overflow = 'auto'
  }
}
</script>

<style>
  .overlay {
    position: fixed;
    display: flex;
    width: 100%;
    height: 100%;
    top:0;
    left:0;
    bottom:0;
    right:0;
    background: rgba(0, 0, 0, 0.5)
  }
  .inner {
    background: #fff;
    color: "#222";
  }
</style>
