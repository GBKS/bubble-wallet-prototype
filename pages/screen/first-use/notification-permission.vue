<script setup>
import transition from '@/helpers/transition.js'
import { useStateStore } from "@/stores/state.js"

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])

const stateStore = useStateStore()
</script>

<template>
  <KitScreen class="notification-permission">
    <template v-if="stateId == 'first-use/notification-permission' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/unit-format?t=slide-right"
      />
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
      <div class="bottom">
        <KitButton
          :label="state.button.label"
          :to="state.button.to"
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

.notification-permission {
  background-color: #EC7129;
  background-image: url('/images/coins.jpg');
  background-size: cover;

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

  ::v-deep(.bottom) {
    .kit-button {
      &:first-child {
        background-color: #ffffff;
        color: #8E1C01;
      }

      &:last-child {
        border: 1px solid #ffffff;
        color: #ffffff;
        background-color: rgba(black, 0.05);
        backdrop-filter: blur(25px);
      }
    }
  }

  @include retina {
    background-image: url('/images/coins@2x.jpg');
  }
}

</style>