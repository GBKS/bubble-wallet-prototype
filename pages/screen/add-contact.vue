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
    <template v-if="stateId == 'add-contact' && state">
      <KitTopBar
        buttonLeftLabel="Cancel"
        buttonLeftTo="/screen/contacts?t=slide-down"
      />
      <h1>Add contact</h1>
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.add-contact {
  h1 {
    text-align: center;
    padding: 50px 0px;
    color: var(--neutral-5);
  } 
}

</style>