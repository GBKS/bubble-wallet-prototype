<script setup>
import Icons from '@/helpers/icons.js'

const props = defineProps([
  'info'
])
</script>

<template>
    <button class="send-key">
      <span v-if="info.label">{{ info.label }}</span>
      <span v-if="info.icon" v-html="Icons[info.icon]" />
    </button>
</template>

<style scoped lang="scss">

.send-key {
  flex-basis: 26%;
  flex-grow: 1;
  aspect-ratio: 1.5;
  display: flex;
  justify-content: center;
  align-items: center;
  // background-color: #f8f8f8;

  span {
    font-size: 21px;
    color: var(--front);

    ::v-deep(svg) {
      width: 16px;
      height: 16px;
    }
  }
}

</style>
