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
  <KitScreen class="backrooms">
    <template v-if="stateId == 'backrooms' && state">
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
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

.backrooms {
  background-color: #E68EB6;
  background-image: url('/images/backrooms.jpg');
  background-size: cover;

  ::v-deep(.header) {
    margin-top: 75px;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    padding-bottom: 40px;

    .top {
      flex-grow: 1;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding-top: 0;
      mix-blend-mode: normal;
      
      h1, p {
        color: #ffffff;
        text-shadow: 0 5px 10px rgba(0, 0, 0, 0.25);
      }

      h1 {
        font-size: 36px;
      }

      p {
        font-size: 24px;
        font-weight: 500;
      }
    }
  }

  ::v-deep(.bottom) {
    flex-grow: 0;

    .kit-button {
      &:first-child {
        background-color: #ffffff;
        color: #8E1C01;
      }
    }
  }

  @include retina {
    background-image: url('/images/backrooms@2x.jpg');
  }
}

</style>