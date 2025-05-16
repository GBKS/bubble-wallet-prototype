<script setup>
import { useStateStore } from "@/stores/state.js"
import transition from '@/helpers/transition.js'
import Icons from '@/helpers/icons.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])

const stateStore = useStateStore()
const route = useRoute()

const classObject = computed(() => {
  const c = [
    'transaction',
    '-'+(amountIsNegative.value ? 'send' : 'receive')
  ]

  return c.join(' ')
})

const walletTransactions = computed(() => {
  return stateStore.transactions[stateStore.activeWalletId]
})

const transaction = computed(() => {
  let result = walletTransactions.value[0]

  const transactionId = route.query.id
  if(transactionId) {
    const transactionIndex = walletTransactions.value.findIndex(item => {
      return item.id == transactionId
    })

    if(transactionIndex > -1) {
      result = walletTransactions.value[transactionIndex]
    }
  }

  return result
})

const amountIsNegative = computed(() => {
  return transaction.value.amount < 0
})

const icon = computed(() => {
  return amountIsNegative.value ? Icons.roundedArrowUp : Icons.roundedArrowDown
})
</script>

<template>
  <KitScreen :class="classObject">
    <template v-if="stateId == 'transaction' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftTo="/screen/activity?t=slide-right"
      />

      <div class="amount">
        <div class="icon" v-html="icon" />
        <KitBalance
          class="-title-2"
          :amount="transaction.amount"
        />
      </div>

      <div class="info">
        <h3 class="-title-5">Note</h3>
        <p class="-body-5">{{ transaction.title }}</p>

        <h3 class="-title-5">Date</h3>
        <p class="-body-5">{{ transaction.date }}</p>

        <h3 class="-title-5">Address</h3>
        <KitAddress :address="transaction.address" />
      </div>
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.transaction {
  .amount {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 20px;
    padding-top: 50px;
    padding-bottom: 30px;

    .icon {
      width: 60px;
      height: 60px;
      display: flex;
      justify-content: center;
      align-items: center;
      line-height: 0;
      border-radius: 100px;

      ::v-deep(svg) {
        width: 20px;
        height: 20px;
        color: white;
      }
    }
  }

  .info {
    display: flex;
    flex-direction: column;
    margin-top: 40px;
    padding-left: 15px;
    padding-right: 15px;

    h3 {
      color: var(--neutral-7);
      font-weight: 500;

      & + p {
        margin-top: 5px;
      }
    }

    p {
      color: var(--neutral-9);
      font-weight: 400;

      & + h3 {
        margin-top: 20px;
      }
    }
  }

  &.-send {
    .amount {
      .icon {
        background-color: var(--neutral-9);
      }
    }
  }

  &.-receive {
    .amount {
      .icon {
        background-color: var(--green);
      }
    }
  }
}

</style>