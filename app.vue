<script setup>
import mitt from 'mitt'
import { storeToRefs } from 'pinia'
import StateHelper from '@/helpers/state-helper.js'
import { useStateStore } from "@/stores/state.js"

// First use
import CoverScreen from '@/data/screens/first-use/cover.json'
import BackupScreen from '@/data/screens/first-use/backup.json'
import BackupOptionsScreen from '@/data/screens/first-use/backup-options.json'
import OkScreen from '@/data/screens/first-use/ok.json'
import LspScreen from '@/data/screens/first-use/lsp.json'
import LspOptionsScreen from '@/data/screens/first-use/lsp-options.json'
import LspDetailsScreen from '@/data/screens/first-use/lsp-details.json'
import BubbleAddressIntroScreen from '@/data/screens/first-use/bubble-address-intro.json'
import BubbleAddressNameScreen from '@/data/screens/first-use/bubble-address-name.json'
import BubbleAddressDoneScreen from '@/data/screens/first-use/bubble-address-done.json'
import UnitFormatScreen from '@/data/screens/first-use/unit-format.json'
import FeaturesScreen from '@/data/screens/first-use/features.json'
import DepositScreen from '@/data/screens/first-use/deposit.json'
import NotificationPermissionScreen from '@/data/screens/first-use/notification-permission.json'
import BackroomsScreen from '@/data/screens/backrooms.json'

import AboutScreen from '@/data/screens/about.json'
import StorageSettingsScreen from '@/data/screens/storage-settings.json'
import DeveloperOptionsScreen from '@/data/screens/developer-options.json'
import ActivityScreen from '@/data/screens/activity.json'
import SendScreen from '@/data/screens/send.json'
import SendReviewScreen from '@/data/screens/send-review.json'
import ReceiveScreen from '@/data/screens/receive.json'
import ContactsScreen from '@/data/screens/contacts.json'
import ContactScreen from '@/data/screens/contact.json'
import AddContactScreen from '@/data/screens/add-contact.json'

// Settings
import SettingsScreen from '@/data/screens/settings.json'
import TransactionScreen from '@/data/screens/transaction.json'
import ConnectionSettingsScreen from '@/data/screens/connection-settings.json'
import DisplaySettingsScreen from '@/data/screens/display-settings.json'

// Wallet creation and import
import AddWalletScreen from '@/data/screens/add-wallet.json'
import ImportWalletScreen from '@/data/screens/import-wallet.json'
import CreateWalletSingleKeyInfoScreen from '@/data/screens/create/single-key-info.json'
import CreateWalletNameScreen from '@/data/screens/create/name.json'
import CreateWalletPasswordScreen from '@/data/screens/create/password.json'
import CreateWalletBackupScreen from '@/data/screens/create/backup.json'
import CreateWalletSuccessScreen from '@/data/screens/create/success.json'

const Screens = {
  "first-use/cover": CoverScreen,
  "first-use/backup": BackupScreen,
  "first-use/backup-options": BackupOptionsScreen,
  "first-use/ok": OkScreen,
  "first-use/lsp": LspScreen,
  "first-use/lsp-options": LspOptionsScreen,
  "first-use/lsp-details": LspDetailsScreen,
  "first-use/bubble-address-intro": BubbleAddressIntroScreen,
  "first-use/bubble-address-name": BubbleAddressNameScreen,
  "first-use/bubble-address-done": BubbleAddressDoneScreen,
  "first-use/unit-format": UnitFormatScreen,
  "first-use/features": FeaturesScreen,
  "first-use/deposit": DepositScreen,
  "first-use/notification-permission": NotificationPermissionScreen,
  "backrooms": BackroomsScreen,
  // "activity": ActivityScreen,
  // "transaction": TransactionScreen,
  // "send": SendScreen,
  // "send-review": SendReviewScreen,
  // "receive": ReceiveScreen,
  // "contacts": ContactsScreen,
  // "contact": ContactScreen,
  // "add-contact": AddContactScreen,
  // "settings": SettingsScreen,
  // "display-settings": DisplaySettingsScreen,
  // "connection-settings": ConnectionSettingsScreen,
  // "storage-settings": StorageSettingsScreen,
  // "about": AboutScreen,
  // "developer-options": DeveloperOptionsScreen,
  // "add-wallet": AddWalletScreen,
  // "import-wallet": ImportWalletScreen,
  // "create/single-key-info": CreateWalletSingleKeyInfoScreen,
  // "create/name": CreateWalletNameScreen,
  // "create/password": CreateWalletPasswordScreen,
  // "create/backup": CreateWalletBackupScreen,
  // "create/success": CreateWalletSuccessScreen
}

const state = useStateStore()
const route = useRoute()
const activeProtoNavTab = ref('screens')
const activeProtoNavId = ref(null)
const activeContentStateId = ref(null)
const { showNav, theme } = storeToRefs(state)
const contentState = ref(null)
const preparedContentState = ref(null)
const mobileOnDesktop = ref(true)
const screenSize = ref('desktop')
const pageWrap = ref(null)
const isMounted = ref(false)

function prepScreens() {
  let i, k
  for(i in Screens) { 
    Screens[i].to = '/screen/'+i

    for(k in Screens[i].states) {
      Screens[i].states[k].to = '/screen/'+i+'?state='+k
    }
  }
}
prepScreens()

function updateFromRoute() {
  // showNav.value = route.path !== '/'

  const bits = route.path.substr(1).split('/')

  const screenId = bits.length > 1 ? bits.slice(1).join('/') : null

  if(bits[0] == 'screen') {
    activeProtoNavTab.value = 'screens'
    // contentState.value = Screens[bits[1]]
    contentState.value = Screens[screenId]

    prepContentState(Screens[screenId], route.query.state)
  }

  activeContentStateId.value = route.query.state || 'default'

  activeProtoNavId.value = screenId

  if(route.query.theme) {
    state.theme = route.query.theme
    updateTheme()
  }
}

watch(() => route.path, () => { updateFromRoute() })
watch(() => route.query, () => { updateFromRoute() })

const navInfo = computed(() => {
  return Screens
})

function detectTheme() {
  if(!state.theme) {
    const prefersDark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches
    
    if(prefersDark) {
      state.theme = 'dark'
    } else {
      state.theme = 'light'
    }
  }
  
  updateTheme()
}

function toggleTheme(value) { 
  state.theme = state.theme == 'light' ? 'dark' : 'light'
  
  updateTheme()
}

function updateTheme() {
  const themeColorMeta = document.getElementById('themeColorMeta')
  if(state.theme == 'dark') {
    document.body.classList.add('--theme-dark')
    themeColorMeta.content = '#000000'
  } else {
    document.body.classList.remove('--theme-dark')
    themeColorMeta.content = '#FFFFFF'
  }
}

function updateScreenSize() {
  if(pageWrap.value) {
    const sizes = [
      'huge',
      'large',
      'medium',
      'small'
    ]

    const widths = [
      1600,
      1280,
      1024,
      640
    ]

    const width = pageWrap.value.offsetWidth

    let result = 'huge'
    for(let i=0; i<sizes.length; i++) {
      if(width < widths[i]) {
        result = sizes[i]
      }
    }
    screenSize.value = result
  }
}

onBeforeMount(() => {
  StateHelper.init()
})

onMounted(() => {
  if(process.client) {
    window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', detectTheme)

    detectTheme()

    window.addEventListener('resize', updateScreenSize)
    updateScreenSize()

    state.isInStandaloneMode = window.matchMedia('(display-mode: standalone)').matches || window.navigator.standalone === true;
  }

  const emitter = mitt()
  window.emitter = emitter

  updateFromRoute()

  window.emitter.on('update-state', onUpdateState)

  isMounted.value = true
})

function onUpdateState(value) {
  contentState.value = value
}

function changeActiveProtoNavTab(value) { 
  activeProtoNavTab.value = value
}

function toggleProtoNav() {
  state.showNav = !state.showNav
}

function prepContentState(base, stateId) {
  let result = JSON.parse(JSON.stringify(base.states.default));

  if(stateId) {
    const activeState = JSON.parse(JSON.stringify(base.states[stateId]));

    if(activeState) {
      copyProps(activeState, result)
    }
  }

  result.nav = base.nav

  preparedContentState.value = result
}

function copyProps(one, two) {
  for(let key in one) {
    let item = one[key]

    if(typeof item == 'object') {
      copyProps(one[key], two[key])
    } else {
      two[key] = one[key]
    }
  }
}

const contentClass = computed(() => {
  const c = ['content-wrap']

  if(mobileOnDesktop.value) c.push('-mobile')

  return c.join(' ')
})

const showPage = computed(() => {
  let result = true

  if(route.path != '/' && !preparedContentState.value) {
    result = false
  }

  return result
})
</script>

<template>
  <div id="app">
    <client-only>
      <ProtoNavMain
        :activeTab="activeProtoNavTab"
        :activeId="activeProtoNavId"
        :activeStateId="activeContentStateId"
        :info="navInfo"
        :state="contentState"
        :theme="theme"
        :showNav="showNav"
        :isMobile="mobileOnDesktop"
        @changeTab="changeActiveProtoNavTab"
        @toggleTheme="toggleTheme"
        @toggleNav="toggleProtoNav"
      />
    </client-only>
    <div :class="contentClass">
      <div class="content" id="prototype-container">
        <div class="page-wrap" ref="pageWrap">
          <NuxtLayout>
            <NuxtPage
              v-if="showPage"
              :stateId="activeProtoNavId"
              :state="preparedContentState"
              :screenSize="screenSize"
            />
          </NuxtLayout>
        </div>
        <template v-if="isMounted">
          <KitContextMenu />
          <KitTooltip />
          <UiWalletModal />
        </template>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">

#app {
  display: flex;
  min-height: 100dvh;
  position: relative;

  > .content-wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    flex-grow: 1;
    width: 100%;

    > .content {
      background-color: var(--neutral-0);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      position: relative;
      flex-grow: 1;
      width: 100%;
      container-type: size;

      .page-wrap {
        position: relative;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        position: relative;
        flex-grow: 1;
        width: 100%;
        overflow-y: scroll;
      }
    }
  }

  @include media-query(medium-up) {
    > .content-wrap {
      background-color: var(--neutral-2);
      border-left: 1px solid var(--neutral-4);

      &.-mobile {
        > .content {
          border-radius: 20px;
          max-width: 393px;
          max-height: 852px;
          box-shadow: 0 30px 60px -15px rgba(black, 0.15);
          overflow: hidden;
        }
      }
    }
  }
}

</style>