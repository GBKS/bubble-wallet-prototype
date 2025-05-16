<script setup>
const props = defineProps([
  'active',
  'theme',
  'showNav'
])

const emit = defineEmits([
  'change',
  'toggleTheme',
  'toggleNav'
])

const tabData = [
  // {
  //   id: 'screens',
  //   name: 'Screens'
  // },
  // {
  //   id: 'flows',
  //   name: 'Flows'
  // },
  // {
  //   id: 'state',
  //   name: 'State'
  // }
]
</script>

<template>
  <div class="tabs">
    <ProtoNavTab
      v-for="item in tabData"
      :key="item.id"
      :info="item"
      :active="active"
      @click="emit('change', item.id)"
    />
    <ProtoNavTheme
      :theme="theme"
      @click="emit('toggleTheme')"
    />
    <ProtoNavToggle
      :showNav="showNav"
      @click="emit('toggleNav')"
    />
  </div>
</template>

<style scoped lang="scss">

.tabs {
  display: flex;
  gap: 5px;
  padding: 10px;
  border-bottom: 1px solid var(--neutral-3);
}

</style>
