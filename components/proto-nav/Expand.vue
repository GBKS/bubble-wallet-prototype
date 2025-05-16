<script setup>
import Icons from '@/helpers/icons.js'

const props = defineProps([
  'theme'
])

const icon = computed(() => {
  return Icons.caretRight
})
</script>

<template>
  <button title="Toggle prototype navigation">
    <div />
    <div />
    <div />
  </button>
</template>

<style scoped lang="scss">

button {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  gap: 4px;
  color: var(--neutral-9);
  transition: all 100ms $ease;
  cursor: pointer;

  > div {
    width: 16px;
    height: 1.5px;
    background-color: var(--neutral-9);
    border-radius: 2px;
    transition: all 100ms $ease;

    &:last-child {
      width: 12px;
    }
  }

  &:hover {
    background-color: var(--neutral-4);
  }

  &.-active {
    background-color: var(--neutral-4);
  }

  ::v-deep(svg) {
    width: 12px;
    height: 12px;
    transition: all 100ms $ease;
  }

  @include media-query(small) {
    position: fixed;
    top: 50%;
    left: 10px;
    transform: translateY(-50%);
    padding: 10px;
    background-color: rgba(black, 0.5);
    backdrop-filter: blur(10px);
    border-radius: 10px;
    color: var(--neutral-0);
    z-index: 1000;

    > div {
      background-color: white;
    }

    &:hover {
      background-color: black;
    }
  }

  @include media-query(medium-up) {
    padding: 5px;
  }
}

</style>
