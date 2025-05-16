<script setup>
const props = defineProps([
  'activeTab',
  'activeId',
  'activeStateId',
  'info',
  'state',
  'theme',
  'showNav'
])

const emit = defineEmits([
  'changeTab', 
  'toggleTheme', 
  'toggleNav'
])

const showState = computed(() => {
  return props.activeTab == 'state'
})

function changeTab(value) { emit('changeTab', value) }

const classObject = computed(() => {
  const c = [
    'main',
    '-'+props.activeTab,
    '-'+(props.showNav ? 'expanded' : 'collapsed')
  ]

  return c.join(' ')
})
</script>

<template>
  <div :class="classObject">
    <template v-if="showNav">
      <ProtoNavTabs
        :active="activeTab"
        :theme="theme"
        :showNav="showNav"
        @change="changeTab"
        @toggleTheme="emit('toggleTheme')"
        @toggleNav="emit('toggleNav')"
      />
      <ProtoNavList
        v-if="!showState"
        :info="info"
        :activeId="activeId"
        :activeStateId="activeStateId"
      />
      <ProtoNavState
        v-if="showState"
        :info="state"
      />
    </template>
    <template v-if="!showNav">
      <ProtoNavExpand
        :showNav="showNav"
        @click="emit('toggleNav')"
      />
    </template>
  </div>
</template>

<style scoped lang="scss">

.main {
  background-color: var(--neutral-1);
  position: relative;
  z-index: 1;

  &.-expanded {
    flex-basis: 350px;
    flex-grow: 0;
    flex-shrink: 0;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    min-width: 0;
  }

  &.-collapsed {
    display: flex;
    flex-direction: column;
  }

  @include media-query(small) {
    position: absolute;
    top: calc(env(safe-area-inset-top) + 15px);
    left: 15px;
    z-index: 999;

    &.-expanded {
      width: calc(100vw - 30px);
      height: calc(100dvh - env(safe-area-inset-top) - 30px);
      border-radius: 15px;
      overflow-y: scoll;
    }
  }

  @include media-query(medium-up) {
    &.-expanded {
      max-height: 100dvh;
    }
  }
}

</style>
