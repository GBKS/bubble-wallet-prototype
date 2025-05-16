<script setup>
import { useStateStore } from "@/stores/state.js"
import Icons from '@/helpers/icons.js'

const props = defineProps([
  'label',
  'amount',
  'balance',
  'required',
  'disabled'
])

const emit = defineEmits([
  'change',
  'enableSendMax',
  'validate'
])

const stateStore = useStateStore()
const amountValue = ref(null)
const hasFocus = ref(false)
const error = ref(null)
const uniqueId = 'input_' + Math.round(Math.random() * 1000000)

watch(() => props.amount, (newValue) => {
  updateAmountValue(newValue)
})

watch(() => stateStore.balanceDisplayMode, (newValue) => {
  updateAmountValue(props.amount)
})

function updateAmountValue(newValue) {
  const adjustedValue = stateStore.balanceDisplayMode == 'satoshi' ? newValue : newValue / 100000000
  amountValue.value = adjustedValue
}

const placeholder = computed(() => {
  return stateStore.balanceDisplayMode == 'satoshi' ? '0' : '0.00 000 000'
})

function changeValue(event) {
  console.log('changeValue', event.target.value, parseFloat(event.target.value))
  emit('change', parseFloat(event.target.value))
}

function toggleUnit() {
  stateStore.balanceDisplayMode = stateStore.balanceDisplayMode == 'satoshi' ? 'bitcoin' : 'satoshi'
}

const toggleLabel = computed(() => {
  return stateStore.balanceDisplayMode == 'satoshi' ? 'sats' : '₿' 
})

function setFocus() {
  hasFocus.value = true
}

function removeFocus() {
  hasFocus.value = false

  validate()
}

function validate() {
  if(props.balance) {
    if(amountValue.value > props.balance) {
      error.value = 'insufficient-funds'
    } else {
      error.value = null
    }
  }

  emit('validate', error.value)
}

const hasContent = computed(() => {
  return amountValue.value !== null
})

const classObject = computed(() => {
  const c = ['receive-amount-input']

    if(hasContent.value) {
    c.push('-has-content')
    }

    if(hasFocus.value) {
    c.push('-has-focus')
    }

  return c.join(' ')
})

const step = computed(() => {
  return stateStore.balanceDisplayMode == 'satoshi' ? '1' : '0.00000001'
})

function sendMax() {
  emit('enableSendMax')
}

onBeforeMount(() => {
    amountValue.value = props.amount
})
</script>

<template>
    <div :class="classObject">
      <div class="content">
        <div class="left">
          <label
              class="-body-5"
              :for="uniqueId"
          >{{ label }}</label>
          <input
              class="-body-5"
              type="number"
              :id="uniqueId"
              :value="amountValue"
              :placeholder="placeholder"
              :step="step"
              :required="required"
              :disabled="disabled"
              @focus="setFocus"
              @blur="removeFocus"
              @change="changeValue"
          />
        </div>
        <button
          class="toggle"
          @click="toggleUnit"
        >
          <p class="-body-5">{{ toggleLabel }}</p>
          <div v-html="Icons.flip" />
        </button>
      </div>
      <div class="error" v-if="error">
        <span class="icon" v-html="Icons.alert" />
        <template v-if="error == 'insufficient-funds'">
          <p class="-body-6">Balance: <KitBalance 
            :amount="balance" 
            theme="light"
            tag="span"
            :unit="stateStore.balanceDisplayMode"
          />.</p>
          <button
            class="-body-6"
            @click="sendMax"
          >Send max</button>.
        </template>
      </div>
    </div>
</template>

<style scoped lang="scss">

.receive-amount-input {
    padding: 10px 0;
    display: flex;
    flex-direction: column;
    gap: 5px;

    .content {
      display: flex;
      gap: 5px;


      .left {
        display: flex;
        align-items: center;
        gap: 5px;
        flex-basis: 10px;
        flex-grow: 1;
        min-width: 0;

        label {
          color: var(--neutral-9);
          width: 110px;
        }

        input {
          appearance: none;
          border-width: 0;
          background-color: transparent;
          padding: 0;
          -moz-appearance: textfield;
          color: var(--neutral-9);
          box-sizing: border-box;
          min-width: 0;
          flex: 1;

          &::placeholder {
            color: var(--neutral-5);
          }

          /* Chrome, Safari, Edge, Opera */
          &::-webkit-outer-spin-button,
          &::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
          }

          &:focus-visible {
              outline: none;
          }
        }

        &:hover {
          input {
            color: var(--primary);
          }
        }
      }

      .toggle {
        display: flex;
        align-items: center;
        gap: 5px;
        color: var(--neutral-7);
        flex: 0 0 auto;
        padding: 3px 9px;
        border-radius: 5px;

        > * {
          flex-shrink: 0;
        }

        > div {
          line-height: 0;

          ::v-deep(svg) {
            width: 18px;
            height: 18px;
          }
        }

        &:hover {
          color: var(--primary);
          cursor: pointer;
          background-color: var(--neutral-2);

          p {
            color: var(--primary);
          }
        }
      }
    }

    .error {
      display: flex;
      align-items: center;
      // gap: 10px;

      .icon {
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: var(--red);
        width: 12px;
        height: 12px;
        border-radius: 2px;
        margin-right: 10px;

        ::v-deep(svg) {
          width: 8px;
          height: 8px;
          color: white;
        }
      }

      p {
        color: var(--red);

        span {
          color: var(--red);
        }
      }

      button {
        margin-left: 5px;
        text-decoration: underline;
        color: var(--red);

        &:hover {
          color: var(--primary);
          cursor: pointer;
        }
      }
    }

    &.-has-content,
    &.-has-focus {
      .content {
          .left label {
              // color: var(--neutral-7);
          }
        }
    }

    @include container(small) {
      .content {
        .left {
          flex-direction: column;
        }
      }
    }

    @include container(medium-up) {
      
    }
}

</style>
