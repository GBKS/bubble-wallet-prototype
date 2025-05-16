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
    <template v-if="stateId == 'contacts' && state">
      <KitTopBar
        buttonRightIcon="plus"
        buttonRightTo="/screen/add-contact?t=slide-up"
      />

      <div class="list">
        <h1>Contacts</h1>
      </div>

      <NavMobileTabs v-if="state && (state.nav === true || state.nav === 'mobile')" />
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.contacts {
  .list {
    flex-grow: 1;

    h1 {
      text-align: center;
      padding: 50px 0px;
      color: var(--neutral-5);
    }
  }
}

</style>