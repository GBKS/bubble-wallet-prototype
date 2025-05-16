<script setup>
import transition from '@/helpers/transition.js'
import Icons from '@/helpers/icons.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])

const inputModel = ref('')
const nameIsValid = ref(null)
const isChecking = ref(false)
let timer = null
let checkTimer = null

const namePreview = computed(() => {
  let result = 'username'

  if(inputModel.value.length >= 3) {
    result = inputModel.value
  }

  return '₿' + result + '@bubble.com'
})

const previewClasses = computed(() => {
  const c = ['-title-5']

  if(inputModel.value.length < 3) {
    c.push('-disabled')
  }

  return c
})

const checkingClasses = computed(() => {
  const c = ['-checking']

  if(isChecking.value) {
    c.push('-active')
  }

  return c
})

const inputChange = (e) => {
  startTimer()
}

const inputKeyUp = (e) => {
  startTimer()
}

const stopTimer = () => {
  if (timer) {
    clearTimeout(timer)
    timer = null
  }
}

const startTimer = () => {
  nameIsValid.value = null

  stopCheckTimer()
  stopTimer()
  timer = setTimeout(() => {
    onTimer()
  }, 500)
}

const onTimer = () => {
  isChecking.value = true
  startCheckTimer()
}

const stopCheckTimer = () => {
  if (checkTimer) {
    clearTimeout(checkTimer)
    checkTimer = null
  }
}

const startCheckTimer = () => {
  stopCheckTimer()
  checkTimer = setTimeout(() => {
    onCheckTimer()
  }, 1500)
}

const onCheckTimer = () => {
  isChecking.value = false
  nameIsValid.value = Math.random() > 0.5
}
</script>

<template>
  <KitScreen class="bubble-address-name">
    <template v-if="stateId == 'first-use/bubble-address-name' && state">
      <KitTopBar
        buttonLeftIcon="caretLeft"
        buttonLeftAriaLabel="Back"
        buttonLeftTo="/screen/first-use/bubble-address-intro?t=slide-right"
      />
      <img
        src="/images/peeps.png"
        srcset="/images/peeps.png 1x, /images/peeps@2x.png 2x"
        alt=""
      >
      <KitHeader
        :title="state.title"
      />
      <div class="field">
        <input
          type="text"
          autocomplete="off"
          autocorrect="off"
          pattern="[a-z0-9_]*"
          placeholder="Enter your name"
          v-model="inputModel"
          @change="inputChange"
          @keyup="inputKeyUp"
        />

        <p>Your address will be</p>
        <p :class="previewClasses">{{ namePreview }}</p>
        <p class="-checking" v-if="isChecking">Checking availability</p>
        <p class="-available" v-if="nameIsValid">Name is available</p>
        <p class="-not-available" v-if="nameIsValid === false">Choose a different name</p>
      </div>
      <div class="bottom">
        <KitButton
          :label="state.button.label"
          :to="state.button.to"
          :disabled="!nameIsValid"
        />
      </div>
    </template>
  </KitScreen>
</template>

<style scoped lang="scss">

.bubble-address-name {
  img {
    margin-top: 20px;
    width: 200px;
    height: 200px;
    border-radius: 25px;
  }

  .field {
    width: 100%;
    margin-top: 20px;
    padding-left: 20px;
    padding-right: 20px;

    input {
      width: 100%;
      appearance: none;
      border-width: 0;
      background-color: var(--neutral-1);
      border-radius: 25px;
      padding: 10px;
      text-align: center;
      font-size: 21px;
      font-weight: 500;

      &::placeholder {
        color: var(--neutral-5);
      }
    }

    p {
      text-align: center;

      &:nth-child(2) {
        margin-top: 30px;
      }

      &:nth-child(3) {
        margin-top: 5px;
        color: var(--neutral-9);
        transition: all 150ms $ease;

        &.-disabled {
          color: var(--neutral-4);
        }
      }

      &.-checking {
        margin-top: 15px;
        transition: all 150ms $ease;
      }

      &.-available {
        margin-top: 15px;
        transition: all 150ms $ease;
        justify-self: center;
        background-color: var(--green);
        color: white;
        border-radius: 100px;
        padding: 3px 15px 4px 15px;
      }

      &.-not-available {
        margin-top: 15px;
        transition: all 150ms $ease;
        color: var(--primary);
      }
    }
  }
}

</style>