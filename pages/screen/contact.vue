<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])
</script>

<template>
  <KitScreen class="contacts">
    <template v-if="stateId == 'contact' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftTo="/screen/activity?t=slide-right"
      />
      <h1>Contact</h1>
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.contact {
  h1 {
    text-align: center;
    padding: 50px 0px;
    color: var(--neutral-5);
  } 
}

</style>