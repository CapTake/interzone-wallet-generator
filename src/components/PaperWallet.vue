<template>
  <div class="ba center bg-black-90" style="width: 999px; height: 666px;">
    <div class="flex w-100 h-50 bb-1 bl-0 bt-0 br-0 b--dashed b--silver">
      <div class="flex flex-column justify-between w-third h-100 bb-0 bl-0 bt-0 br-1 b--dashed b--silver">
      </div>
      <div class="flex items-center justify-center w-third h-100 bb-0 bl-0 bt-0 br-1 b--dashed b--silver">
        <div class="flex flex-column w-90 bg-white br1" style="transform: rotate(180deg)">
          <div class="tc pv2 mt3 w-100 f3 ttu white bg-red">Private key</div>
          <img :src="privq" class="center mt3" style="width:150px; height:150px"/>
          <div class="tc pv3 ph4 code w-100 f6" style="word-break: break-all; overflow-wrap: break-word; word-wrap: break-word;">{{ priv }}</div>
        </div>
      </div>
    </div>
    <div class="flex w-100 h-50">
      <div class="flex items-center justify-center w-third h-100 bb-0 bl-0 bt-0 br-1 b--dashed b--silver">
        <div class="flex flex-column w-90 bg-white br1">
          <div class="tc pv2 mt3 w-100 f3 ttu white bg-green">Public address</div>
          <img :src="pubq" class="center mt3" style="width:150px; height:150px"/>
          <div class="tc pv3 ph5 code w-100 f6" style="word-break: break-all; overflow-wrap: break-word; word-wrap: break-word;">{{ pub }}</div>
        </div>
      </div>
      <div class="flex items-center justify-center w-third h-100 bb-0 bl-0 bt-0 br-1 b--dashed b--silver">
        <div class="flex flex-column items-center justify-center w-90 bg-light-yellow near-black ph3 br1" style="height:289px">
          <p class="tl f7 lh-copy db">To deposit funds to this paper wallet, send the {{ coin }} ({{ short }}) coins to the public address.</p>
          <p class="tl f7 lh-copy db">DO NOT REVEAL THE PRIVATE KEY Until you are ready to import the balance from this
          wallet to your mobile {{ coin }} wallet, a cryptocurrency client, exchange or online wallet.</p>
          <p class="f7 lh-copy v-btm tj db">
            Date:____________ Amount:___________</p>
          <p class="f7 v-btm tl db">
            Notes:______________________________
          </p>
        </div>
      </div>
      <div class="flex flex-column items-center justify-center w-third h-100">
        <img :src="logo" class="center" style="width:100px; height:100px"/>
        <div class="white f2">{{ coin }}</div>
        <div class="white f6">crypto-currency wallet</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['pub', 'priv', 'coin', 'short', 'logo', 'pubq', 'privq']
}
</script>

<style>

</style>
