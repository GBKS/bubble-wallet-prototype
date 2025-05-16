<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)
</script>

<template>
  <div class="index">
    <h1 class="-title-2">Bubble Wallet prototype</h1>
    <p class="-body-5">This is a prototype playground for Christoph to interactively explore ideas for payment-focused bitcoin wallets. Code is <a href="https://github.com/GBKS/bubble-wallet-prototype" target="_blank">here</a>.<br/><br/>Be aware, what you see may trigger you.<br/><br/>If you're on mobile, <a href="https://support.apple.com/guide/shortcuts/add-a-shortcut-to-the-home-screen-apd735880972/ios" target="_blank">add it to your home screen</a> for best effect.<br/><br/><NuxtLink to="/screen/first-use/cover">View cover screen</NuxtLink></p>
  </div>
</template>

<style scoped lang="scss">

.index {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 10px;
  text-align: center;
  max-width: 600px;
  padding-left: 20px;
  padding-right: 20px;
}

</style>