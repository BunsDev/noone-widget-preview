<script setup lang="ts">
import { onMounted, reactive, ref } from 'vue'
import TestAddress from './components/TestAddress.vue'
import { EvmConnector, IframeManager } from '@noonewallet/widget-communicator'

const evmConnector = ref<EvmConnector | null>(null)
const dataFromIframe = reactive({
  loaded: false,
  address: '',
  balance: '',
  error: '',
  message: '',
  hash: ''
})
onMounted(async () => {
  const iframe = new IframeManager('noone-iframe', 'http://localhost:8080/')
  await iframe.render()
  evmConnector.value = new EvmConnector(iframe)
})

const getAddress = async () => {
  if (!evmConnector.value) return console.error('EvmConnector is not initialized')
  const result = await evmConnector.value?.send({
    chainId: 1,
    method: 'getAddress'
  })
  if (result.success) {
    dataFromIframe.address = result.data
  } else {
    dataFromIframe.error = result.error
  }
}

const showCode = () => {}
</script>

<template>
  <v-container>
    <v-row no-gutters>
      <v-col cols="6">
        <v-sheet>
          <h1>Communicator Preview</h1>
          <v-icon icon="mdi-antenna">Git</v-icon>
        </v-sheet>
        <test-address :connector="evmConnector"></test-address>
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
</style>
