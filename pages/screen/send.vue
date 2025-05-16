<script setup>
import transition from '@/helpers/transition.js'
import { useStateStore } from "@/stores/state.js"

definePageMeta(transition)

const stateStore = useStateStore()
const keysPressed = ref("0")

const props = defineProps([
  'stateId',
  'state'
])

const classObject = computed(() => {
  const c = ['send']

  return c.join(' ')
})

const formattedAmount = computed(() => {
  return parseFloat(keysPressed.value)
})

const enterKey = (key) => {
  console.log('enterKey', key)

  if(key.id == 'back') {
    if(keysPressed.value.length > 0) {
      keysPressed.value = keysPressed.value.slice(0, -1)
    }
  } else if(key.id == 'point') {
    if(!keysPressed.value.includes('.')) {
      keysPressed.value += '.'
    }
  } else if(key.number !== undefined) {
    keysPressed.value += key.number
  }
  console.log('keysPressed', keysPressed.value)
}

function toggleBalanceMode() {
  let mode = 'bitcoin'
  switch(stateStore.balanceDisplayMode) {
    case 'bitcoin':
      mode = 'satoshi'
      break
    case 'satoshi':
      mode = 'bitoshi'
      break
  }
  stateStore.balanceDisplayMode = mode
}
</script>

<template>
  <KitScreen :class="classObject">
    <template v-if="stateId == 'send' && state">
      <div class="top-mobile">
        <KitTopBar
          buttonRightIcon="gearFilled"
          buttonRightTo="/screen/settings?t=slide-up"
        />
      </div>

      <div class="form">
        <div class="amount-input">
          <KitBalance
            class="-body-2"
            :unit="stateStore.balanceDisplayMode"
            :amount="formattedAmount"
            theme="light"
            @click="toggleBalanceMode"
          />
        </div>

        <ScreensSendKeyPad
          @select="enterKey"
        />

        <div class="options">
          <KitButton
            label="Request"
            to="/screen/receive?t=cover-up"
          />
          <KitButton
            icon="qr"
            to="/screen/send-review?t=slide-left"
          />
          <KitButton
            label="Send"
            to="/screen/send-review?t=slide-left"
          />
        </div>
      </div>

      <NavMobileTabs v-if="state && (state.nav === true || state.nav === 'mobile')" />
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.send {
  .top-mobile {
    display: flex;
    justify-content: space-between;
    width: 100%;
    padding: 0 10px;
  }

  .form {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    width: 100%;
    max-width: 560px;
    flex-grow: 1;

    .amount-input {
      flex-grow: 1;
      display: flex;
      justify-content: center;
      align-items: center;

      ::v-deep(.-nz) {
        opacity: 0.35;
      }
    }

    > .options {
      margin-top: 30px;
      display: flex;
      gap: 20px;

      ::v-deep(> *) {
        &:first-child,
        &:last-child {
          flex-basis: 34%;
          flex-grow: 1;
        }
      }
    }
  }

  &.-review {
    background-color: rgba(var(--blue-rgb), 0.15);
  }

  @include container(small) {
    .form {
      flex-direction: column;
      padding: 15px 15px;

      .form-header {
        margin-bottom: 15px;
      }
    }
  }

  @include container(medium-up) {
    .top-mobile {
      display: none;
    }

    .form {
      flex-wrap: wrap;
      column-gap: 40px;
      padding: 30px 20px;

      .form-header {
        margin-bottom: 30px;
      }
    }
  }
}

</style>