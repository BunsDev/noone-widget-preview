<script setup lang="ts">
import {onMounted, reactive, ref} from 'vue'
import TestAddress from './components/TestAddress.vue'
import TestBalance from './components/TestBalance.vue'
import TestBlockNumber from './components/TestBlockNumber.vue'
import TestNonce from './components/TestNonce.vue'
import TestSignMsg from './components/TestSignMsg.vue'
import TestSignTx from './components/TestSignTx.vue'
import TestChainIds from './components/TestChainIds.vue'
import TestGasPrice from './components/TestGasPrice.vue'
import ChainSelector from './components/ChainSelector.vue'
import {EvmConnector, IframeManager, WalletConnector} from '@noonewallet/widget-communicator'

const evmConnector = ref<EvmConnector | null>(null)
const walletConnector = ref<WalletConnector | null>(null)
const dataFromIframe = reactive({
  loaded: false
})

const lastTriggeredEvent = ref<string>('')
const dev_url = 'http://localhost:8080/'
const prod_url = 'https://crypto-widget.noone.io/'
let chains = reactive([{
  title: 1,
  subtitle: 'Ethereum'
}])
let selectedChain = ref(1)

onMounted(async () => {
  const iframe = new IframeManager('noone-iframe', prod_url)
  dataFromIframe.loaded = await iframe.render()
  evmConnector.value = new EvmConnector(iframe)
  walletConnector.value = new WalletConnector(iframe)
  walletConnector.value.listen('logout', (data) => {
    console.log('trigger - logout', data)
    lastTriggeredEvent.value = 'logout'
  })
  walletConnector.value.listen('login', (data) => {
    console.log('trigger - login', data)
    lastTriggeredEvent.value = 'login'
    setTimeout(() => {
      getChains()
    }, 2000)
  })
  walletConnector.value.listen('switch-wallet', (data) => {
    console.log('trigger - switch-wallet', data)
    lastTriggeredEvent.value = 'switch-wallet'
  })
})

const getChains = async () => {
  const result = await evmConnector.value.send({
    chainId: 1,
    method: 'getChains'
  })
  if (result.success) {
    chains = Object.assign(chains, result.data)
    console.log('chains', chains)
  }
}

const selectChain = (chain) => {
  console.log('selectChain', chain)
  selectedChain.value = chain
}
</script>

<template>
<v-container>
  <v-row no-gutters>
    <v-col cols="6">
      <v-sheet>
        <h1>Communicator Preview</h1>
        <v-icon icon="mdi-antenna">Git</v-icon>
      </v-sheet>
      <v-row align="center">
        <v-col>
          <h3>Iframe loaded: {{ dataFromIframe.loaded }}</h3>
          <h3>Triggered event: {{ lastTriggeredEvent }}</h3>
        </v-col>
        <v-col>
          <p>Project on <a href="https://github.com/noonewallet/noone-widget-preview" target="_blank">Github
            <v-icon icon="mdi-github"></v-icon>
          </a></p>
        </v-col>
      </v-row>
      <chain-selector :chains="chains" @select-chain="selectChain"></chain-selector>
      <test-address :connector="evmConnector" :chain="selectedChain"></test-address>
      <v-divider></v-divider>
      <test-balance :connector="evmConnector" :chain="selectedChain"></test-balance>
      <v-divider></v-divider>
      <test-sign-msg :connector="evmConnector" :chain="selectedChain"></test-sign-msg>
      <v-divider></v-divider>
      <test-sign-tx :connector="evmConnector" :chain="selectedChain"></test-sign-tx>
      <v-divider></v-divider>
      <test-chain-ids :connector="evmConnector" :chain="selectedChain"></test-chain-ids>
      <v-divider></v-divider>
      <test-block-number :connector="evmConnector" :chain="selectedChain"></test-block-number>
      <v-divider></v-divider>
      <test-nonce :connector="evmConnector" :chain="selectedChain"></test-nonce>
      <v-divider></v-divider>
      <test-gas-price :connector="evmConnector" :chain="selectedChain"></test-gas-price>
      <v-divider></v-divider>
    </v-col>
    <v-col>
      <v-sheet class="pa-2 ma-2">
        <div id="noone-iframe"></div>
      </v-sheet>
    </v-col>
  </v-row>
</v-container>
</template>

<style scoped>
main {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #dadada;
}

a:hover {
  text-decoration: none;
}
</style>
