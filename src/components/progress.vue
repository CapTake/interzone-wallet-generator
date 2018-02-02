<template>
  <div>
    <div class="flex w-50 pa1 outline">
      <div :style="{width: percent+'%', transition: 'all 0.2s easing'}" class="pb1 pt1 bg-yellow nowrap">{{ Math.round(percent) + ' %' }}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['percent']
}
</script>

<style>

</style>
