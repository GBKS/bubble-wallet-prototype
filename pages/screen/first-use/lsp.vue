<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])
</script>

<template>
  <KitScreen class="lsp">
    <template v-if="stateId == 'first-use/lsp' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/ok?t=slide-right"
      />
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
      <div  class="illustration">
        <img
          src="/images/lsp.png"
          srcset="/images/lsp.png 1x, /images/lsp@2x.png 2x"
          width="375"
          height="527"
          alt="Electrician holding a power cord"
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

.lsp {
  background: linear-gradient(#C299B0 0%, #C99BB6 100%);

  ::v-deep(.top-bar),
  ::v-deep(.header),
  ::v-deep(.bottom) {
    position: relative;
    z-index: 1;
  }

  ::v-deep(.top-bar) {
    .kit-button {
      color: #ffffff;
    }
  }

  ::v-deep(.header) {
    .top {
      padding-top: 0;
      
      h1, p {
        color: #ffffff;
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
        background-color: #ffffff;
        color: #A06393;
      }

      &:last-child {
        border-color: #ffffff;
        color: #ffffff;
        background-color: rgba(black, 0.05);
        backdrop-filter: blur(25px);
      }
    }
  }
}

</style>