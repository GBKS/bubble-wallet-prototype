<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])
</script>

<template>
  <KitScreen class="bubble-address-intro">
    <template v-if="stateId == 'first-use/bubble-address-intro' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/lsp?t=slide-right"
      />
      <KitHeader
        :title="state.title"
        :description="state.description"
        alt=""
      />
      <div class="illustration">
        <img
          src="/images/bubble-address.png"
          srcset="/images/bubble-address.png 1x, /images/bubble-address@2x.png 2x"
          width="375"
          height="802"
          alt="Bubble address"
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

.bubble-address-intro {
  background: linear-gradient(#887672 0%, #864E43 100%);

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
        color: #53364D;
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