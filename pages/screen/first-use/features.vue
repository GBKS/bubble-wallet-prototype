<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])
</script>

<template>
  <KitScreen class="features">
    <template v-if="stateId == 'first-use/features' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/notification-permission?t=slide-right"
      />
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
      <div  class="illustration">
        <img
          src="/images/features.png"
          srcset="/images/features.png 1x, /images/features@2x.png 2x"
          width="375"
          height="562"
          alt="Box filled with various objects"
        >
      </div>
      <div class="bottom">
        <KitButton
          :label="state.button.label"
          :to="state.button.to"
        />
      </div>
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.features {
  background: linear-gradient(#4F8C81 0%, #92AD99 100%);

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
      background-color: #ffffff;
      color: #3F8175;
    }
  }
}

</style>