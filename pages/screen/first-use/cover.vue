<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])

const clickRestore = () => {
  alert('Nothing to see here... move along!')
}
</script> 

<template>
  <KitScreen class="cover">
    <template v-if="stateId == 'first-use/cover' && state">
      <div class="info">
        <KitHeader
          :title="state.title"
          :description="state.description"
          :big="true"
        />
      </div>
      <div class="bottom">
        <KitButton
          :label="state.button.label"
          :to="state.button.to"
        />
        <KitButton
          :label="state.buttonTwo.label"
          :to="state.buttonTwo.to"
          @click="clickRestore"
        />
      </div>
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.cover {
  background-color: #C7A4B5;
  background-image: url('/images/cover.jpg');
  background-size: cover;

  .note {
    margin-top: 25px;
  }

  .info {
    display: flex;
    align-items: flex-end;
    flex-grow: 1;

    ::v-deep(.header) {
      .top {
        padding-top: 0;
        
        h1, p {
          color: #ffffff;
        }
      }
    }
  }

  ::v-deep(.bottom) {
    flex-grow: 0;
    padding-top: 50px;

    .kit-button {
      &:first-child {
        background-color: #ffffff;
        color: #C65681;
      }

      &:last-child {
        border-color: #ffffff;
        color: #ffffff;
        background-color: rgba(#C65681, 0.1);
        backdrop-filter: blur(25px);
        border: 1px solid white;
      }
    }
  }

  @include retina {
    background-image: url('/images/cover@2x.jpg');
  }
}

</style>