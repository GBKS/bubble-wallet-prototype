<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])
</script>

<template>
  <KitScreen class="ok">
    <template v-if="stateId == 'first-use/ok' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/backup?t=slide-right"
      />
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
      <div  class="illustration">
        <img
          src="/images/ok.png"
          srcset="/images/ok.png 1x, /images/ok@2x.png 2x"
          width="375"
          height="607"
          alt="Thumbs up"
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

.ok {
  background: linear-gradient(#464E78 0%, #45658D 100%);

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
    aspect-ratio: 1.06;
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
      color: #59658C;
    }
  }
}

</style>