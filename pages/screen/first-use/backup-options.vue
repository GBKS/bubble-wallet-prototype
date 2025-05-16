<script setup>
import transition from '@/helpers/transition.js'

definePageMeta(transition)

const props = defineProps([
  'stateId',
  'state'
])

const cloudEnabled = ref(false)
</script>

<template>
  <KitScreen class="backup-options">
    <template v-if="stateId == 'first-use/backup-options' && state">
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
        <div class="device">
          <h4 class="-title-5">1. This device</h4>
          <p>All wallet data will be stored locally on this device. Protect it with Touch ID, Face ID, or a PIN.</p>
        </div>

        <div class="device">
          <h4 class="-title-5">2. iCloud</h4>
          <p>All data will also be regularly stored in your iCloud storage in a secure way.</p>
          <div class="toggle">
            <p>Enable</p>
            <KitToggle
              size="small"
              :active="cloudEnabled"
              @click="cloudEnabled = !cloudEnabled"
            />
          </div>
        </div>

        <div class="device">
          <h4 class="-title-5">3. Manual backup</h4>
          <p>Write down your keys as a way to recover your wallet in case you lose access to this device and iCloud.</p>
          <KitButton
            label="Back up now"
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

.backup-options {
  .list {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    gap: 10px;
    width: 100%;
    max-width: 450px;
    padding-left: 20px;
    padding-right: 20px;

    > div {
      border: 1px solid var(--neutral-4);
      border-radius: 15px;
      padding: 15px;

      h4 {

      }

      p {
        margin-top: 5px;
      }

      .toggle {
        margin-top: 5px;
        display: flex;
        justify-content: space-between;
        align-items: center;

        p {
          color: var(--neutral-9)
        }
      }

      ::v-deep(.kit-button) {
        margin-top: 10px;
        width: 100%;
        font-size: 15px;
        color: var(--primary);
        background-color: var(--neutral-1);
        height: 40px;
      }
    }
  }
}

</style>