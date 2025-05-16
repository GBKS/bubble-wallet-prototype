<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])
</script>

<template>
  <KitScreen class="bubble-address-done">
    <template v-if="stateId == 'first-use/bubble-address-done' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/bubble-address-name?t=slide-right"
      />
      <img
        src="/images/oh-yeah.jpg"
        srcset="/images/oh-yeah.jpg 1x, /images/oh-yeah@2x.jpg 2x"
        alt=""
      >
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
      <div class="bottom">
        <KitButton
          :label="state.button.label"
          :to="state.button.to"
          theme="outline"
        />
        <KitButton
          :label="state.buttonTwo.label"
          :to="state.buttonTwo.to"
        />
      </div>
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.bubble-address-done {
  img {
    margin-top: 20px;
    width: 200px;
    height: 200px;
    border-radius: 100px;
  }

  ::v-deep(.bottom) {
    .kit-button {
      &:first-child {
        border-color: var(--primary);
        color: var(--primary);
      }
    }
  }
}

</style>