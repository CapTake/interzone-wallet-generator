<template>
    <div class="pa1 outline w-100 center">
      <div :style="{width: percent+'%', transition: 'all 0.2s easing'}" class="pb1 pt1 bg-yellow nowrap">{{ Math.round(percent) + ' %' }}</div>
    </div>
</template>

<script>
export default {
  props: ['percent']
}
</script>

<style>

</style>
