<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])

const options = [
  {
    id: 'bitoshi',
    title: 'Simplified'
  },
  {
    id: 'satoshi',
    title: 'Satoshi'
  },
  {
    id: 'bitcoin',
    title: 'Original'
  }
]

const activeId = ref('bitoshi')
</script>

<template>
  <KitScreen class="unit-format">
    <template v-if="stateId == 'first-use/unit-format' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/bubble-address-intro?t=slide-right"
      />
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
      <div class="list">
        <button
          v-for="option in options"
          :key="option.id"
          :frame="true"
          :aria-selected="option.id == activeId"
          :class="option.id == activeId ? '-active' : ''"
          @click="activeId = option.id"
        >
          <div class="left">
            <KitBalance
              class="-title-5"
              amount="1000"
              :unit="option.id"
              theme="dark"
            />
          </div>
          <p class="-title-5">{{ option.title }}</p>
        </button>
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

.unit-format {
  background: linear-gradient(#46507A 0%, #776D9F 50%, #46658D 100%);

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

  .list {
    margin-top: 30px;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    gap: 20px;
    width: 100%;
    max-width: 450px;
    padding-left: 20px;
    padding-right: 20px;

    button {
      display: flex;
      align-items: center;
      border: 1px solid rgba(white, 0.75);
      border-radius: 15px;
      overflow: hidden;
      gap: 15px;

      .left {
        background-color: rgba(white, 0.75);
        color: black;
        width: 175px;
        display: flex;
        align-items: flex-end;
        justify-content: flex-end;
        padding: 15px 15px 15px 5px;

        .balance {
          color: #666A99;

          &.-bitcoin {
            ::v-deep(.-nz) {
              opacity: 0.5;
            }
          }
        }
      }

      > p {
        flex-grow: 1;
        text-align: left;
        color: white;
        opacity: 0.75;
      }

      &:hover {
        cursor: pointer;
      }

      &.-active {
        border-color: white;

        .left {
          background-color: white;
        }

        > p {
          opacity: 1;
        }
      }
    }
  }

  ::v-deep(.bottom) {
    .kit-button {
      background-color: #ffffff;
      color: #666A99;
    }
  }
}

</style>