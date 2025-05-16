<script setup>
const props = defineProps([
  'activeId'
])

const info = {
  // activity: {
  //   name: 'Activity',
  //   icon: 'home',
  //   to: '/screen/activity'
  // },
  send: {
    name: 'Transact',
    icon: 'flip',
    to: '/screen/send'
  },
  activity: {
    name: 'Activity',
    icon: 'list',
    to: '/screen/activity'
  },
  contacts: {
    name: 'contacts',
    icon: 'contact',
    to: '/screen/contacts'
  }
}
</script>

<template>
  <div class="tabs">
    <NavMobileTab
      v-for="(item, id) in info"
      :key="id"
      :id="id"
      :info="item"
      :activeId="activeId"
    />
  </div>
</template>

<style scoped lang="scss">

.tabs {
  // border-top: 1px solid var(--neutral-4);
  display: flex;
  justify-content: stretch;
  align-items: stretch;
  gap: 5px;
  height: 90px;
  padding-bottom: 25px;
  padding-left: 5px;
  padding-right: 5px;
  width: 100%;
  align-self: flex-end;
  flex-shrink: 0;
}

</style>
