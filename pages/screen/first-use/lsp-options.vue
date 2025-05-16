<script setup>
import transition from '@/helpers/transition.js'
import Icons from '@/helpers/icons.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])

const options = [
  {
    id: 'thunder-charge',
    title: 'Thunder Charge',
    tagline: 'We are the best LSP, except for all the others.',
    description: 'Fee rate: 0.5%',
    image: 'thunder-charge',
    default: true
  },
  {
    id: 'lightning-leap',
    title: 'Lightning Leap',
    tagline: 'We are the best LSP, except for all the others.',
    description: 'Fee rate: 0.75%',
    image: 'lightning-leap'
  },
  {
    id: 'current',
    title: 'Current',
    tagline: 'We are the best LSP, except for all the others.',
    description: 'Fee rate: 0.35%',
    image: 'current'
  }
]

const activeId = ref('thunder-charge')
</script>

<template>
  <KitScreen class="lsp-options">
    <template v-if="stateId == 'first-use/lsp-options' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/lsp?t=slide-right"
      />
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
      <div class="list">
        <NuxtLink
          v-for="option in options"
          :key="option.id"
          :frame="true"
          :aria-selected="option.id == activeId"
          :class="option.id == activeId ? '-active' : ''"
          :href="'/screen/first-use/lsp-details?t=slide-left&id='+option.id"
        >
          <img
            :src="'/images/'+option.image+'.jpg'"
            :srcset="'/images/'+option.image+'.jpg 1x, /images/'+option.image+'@2x.jpg 2x'"
            alt=""
          >
          <div class="copy">
            <h3 class="-title-5">{{ option.title }}</h3>
            <p class="-body-6">{{ option.description }}</p>
            <p v-if="option.default" class="-default -title-7">Default</p>
          </div>
          <div
            class="right"
            v-html="Icons.caretRight"
          />
        </NuxtLink>
      </div>
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.lsp-options {
  .list {
    margin-top: 30px;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    width: 100%;
    max-width: 450px;
    padding-left: 20px;
    padding-right: 20px;

    a {
      display: flex;
      align-items: center;
      // border: 1px solid var(--neutral-4);
      // border-radius: 15px;
      // overflow: hidden;
      gap: 15px;
      padding-top: 20px;
      padding-bottom: 20px;
      // padding: 10px;
      text-decoration: none;

      img {
        width: 80px;
        height: 80px;
        border-radius: 5px;
      }

      .copy {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        flex-grow: 1;
        gap: 2px;

        p {
          &.-default {
            margin-top: 5px;
            background-color: var(--neutral-2);
            color: var(--primary);
            border-radius: 20px;
            padding: 3px 8px;
          }
        }
      }

      .right {
        ::v-deep(svg) {
          width: 16px;
          height: 16px;
          color: var(--primary);
        }
      }

      &:hover {
        cursor: pointer;
      }

      &.-active {
        border-color: var(--primary);
      }

      & + a {
        border-top: 1px solid var(--neutral-4);
      }
    }
  }
}

</style>