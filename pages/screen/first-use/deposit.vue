<script setup>
import transition from '@/helpers/transition.js'
import QRCode from "qrcode";

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])

const qrCodeCanvas = ref(null)
const addressGenerated = ref(false)
const amountValue = ref(null)
const nameValue = ref('')
const messageValue = ref('bc1qdgf4kzr5hv6lupdp44z4c6hh8440vm2mcmnlhl')
const addressValue = ref(null)

const bitcoinUrl = computed(() => {
  let result = 'bitcoin:' + addressValue.value

  const bits = []
  
  if(amountValue.value) bits.push('amount=' + amountValue.value)
  if(messageValue.value) bits.push('message=' + messageValue.value)
  if(nameValue.value) bits.push('label=' + nameValue.value)

  if(bits.length) result += '?' + bits.join('&')

  return result
})

function renderCode() {
  console.log('renderCode', bitcoinUrl.value, qrCodeCanvas.value)
  QRCode.toCanvas(qrCodeCanvas.value, bitcoinUrl.value, {
    margin: 0,
    color: {
      dark: '#000000',
      light: '#0000'
    }
  })
}

function renderCodeTimeout() {
  setTimeout(() => {
    renderCode()
  }, 50)
}

const clickShare = () => {
  if(navigator.canShare()) {
    navigator.share({
      title: 'My Bitcoin Address',
      url: bitcoinUrl.value
    })
  } else {
    alert("Oh no, your browser does not support sharing. That's OK, it's just a prototype anyways.")
  }
}

onMounted(() => {
  if(process.client) {
    addressGenerated.value = true
    renderCodeTimeout()
  }
})
</script>

<template>
  <KitScreen class="deposit">
    <template v-if="stateId == 'first-use/deposit' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/features?t=slide-right"
      />
      <KitHeader
        :title="state.title"
        :description="state.description"
      />
      <div  class="illustration">
        <img
          src="/images/frame.png"
          srcset="/images/frame.png 1x, /images/frame@2x.png 2x"
          width="375"
          height="562"
          alt=""
        >
        <div class="qr-code">
          <Transition name="fade" appear>
            <canvas
              v-if="addressGenerated"
              ref="qrCodeCanvas"
              aria-label="QR code of the bitcoin address"
            />
          </Transition>
        </div>
      </div>
      <div class="bottom">
        <KitButton
          :label="state.button.label"
          @click="clickShare"
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

.deposit {
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
    display: flex;
    justify-content: center;
    align-items: center;

    img {
      position: absolute;
      left: 0;
      top: 50%;
      transform: translateY(-50%);
      width: 100%;
      height: auto;
    }

    .qr-code {
      width: 60%;
      height: auto;
      position: relative;
      flex-grow: 0;

      canvas {
        width: 100% !important;
        height: auto !important;
        aspect-ratio: 1;
      }
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