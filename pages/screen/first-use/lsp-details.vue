<script setup>
import transition from '@/helpers/transition.js'
import Icons from '@/helpers/icons.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])

const route = useRoute()

const options = [
  {
    id: 'thunder-charge',
    title: 'Thunder Charge',
    tagline: 'We are the best LSP, except for all the others.',
    description: 'Fee rate: 0.5%',
    image: 'thunder-charge'
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

const details = [
  {
    id: 'fee-rate',
    title: 'Fee Rate',
    description: '1,000 ₿ + 0.4%',
  },
  {
    id: 'channel-duration',
    title: 'Channel duration',
    description: '1 year',
  },
  {
    id: 'minimum-channel-size',
    title: 'Minimum channel size',
    description: '20,000 ₿',
  },
  {
    id: 'url',
    title: 'lightningleap.com',
    icon: 'link',
  }
]

const lsp = computed(() => {
  let result = options[0]

  const lspId = route.query.id
  if(lspId) {
    const lspIndex = options.findIndex(item => {
      return item.id == lspId
    })

    if(lspIndex > -1) {
      result = options[lspIndex]
    }
  }

  return result
})
</script>

<template>
  <KitScreen class="lsp-details">
    <template v-if="stateId == 'first-use/lsp-details' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/lsp-options?t=slide-right"
      />
      <img
        :src="'/images/'+lsp.image+'.jpg'"
        :srcset="'/images/'+lsp.image+'.jpg 1x, /images/'+lsp.image+'@2x.jpg 2x'"
        alt=""
      >
      <KitHeader
        :title="lsp.title"
        :description="lsp.tagline"
      />
      <div class="list">
        <div 
          v-for="detail in details"
          :key="detail.id"
        >
          <h4 class="-body-5">{{ detail.title }}</h4>
          <p v-if="detail.description" class="-body-5">{{ detail.description }}</p>
          <div
            v-if="detail.icon"
            class="right"
            v-html="Icons[detail.icon]"
          />
        </div>
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

.lsp-details {
  img {
    width: 120px;
    height: 120px;
    border-radius: 25px;
  }

  .list {
    margin-top: 30px;
    display: flex;
    flex-direction: column;
    width: 100%;
    max-width: 450px;
    padding-left: 20px;
    padding-right: 20px;

    > div {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 15px 0;

      > div {
        ::v-deep(svg) {
          width: 20px;
          height: 20px;
          color: var(--primary);
        }
      }

      & + div {
        border-top: 1px solid var(--neutral-4);
      }
    }
  }
}

</style>