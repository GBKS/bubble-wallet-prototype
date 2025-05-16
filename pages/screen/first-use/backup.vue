<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])
</script>

<template>
  <KitScreen class="backup">
    <template v-if="stateId == 'first-use/backup' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/cover?t=slide-right"
      />
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
      <div  class="illustration">
        <img
          src="/images/cloud.png"
          srcset="/images/cloud.png 1x, /images/cloud@2x.png 2x"
          width="375"
          height="610"
          alt="Cloud"
        >
      </div>
      <div class="bottom">
        <KitButton
          :label="state.button.label"
          :to="state.button.to"
        />
        <KitButton
          theme="outline"
          :label="state.buttonTwo.label"
          :to="state.buttonTwo.to"
        />
      </div>
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.backup {
  background: linear-gradient(#FCA8CB 0%, #E5D3F0 100%);

  ::v-deep(.top-bar),
  ::v-deep(.header),
  ::v-deep(.bottom) {
    position: relative;
    z-index: 1;
  }

  ::v-deep(.top-bar) {
    .kit-button {
      color: #8D226E;
    }
  }

  ::v-deep(.header) {
    .top {
      padding-top: 0;
      
      h1, p {
        color: #8D226E;
      }
    }
  }

  .illustration {
    position: relative;
    width: 100%;
    flex-grow: 1;
    pointer-events: none;

    img {
      position: absolute;
      left: 0;
      top: 50%;
      transform: translateY(-50%);
      width: 100%;
      height: auto;
    }
  }

  ::v-deep(.bottom) {
    flex-grow: 0;

    .kit-button {
      &:first-child {
        background-color: #8D226E;
        color: #ffffff;
      }

      &:last-child {
        border-color: #8D226E;
        color: #8D226E;
      }
    }
  }
}

</style>