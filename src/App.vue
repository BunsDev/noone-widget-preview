<script setup lang="ts">
import { onMounted, reactive, ref } from 'vue'
import TestAddress from './components/TestAddress.vue'
import TestBalance from './components/TestBalance.vue'
import TestBlockNumber from './components/TestBlockNumber.vue'
import TestNonce from './components/TestNonce.vue'
import { EvmConnector, IframeManager } from '@noonewallet/widget-communicator'

const evmConnector = ref<EvmConnector | null>(null)
const dataFromIframe = reactive({
  loaded: false
})

onMounted(async () => {
  const iframe = new IframeManager('noone-iframe', 'http://localhost:8080/')
  dataFromIframe.loaded = await iframe.render()
  evmConnector.value = new EvmConnector(iframe)
})
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
            <h2>Iframe loaded: {{ dataFromIframe.loaded }}</h2>
          </v-col>
          <v-col>
            <p>Project on <a href="https://github.com/noonewallet/noone-widget-preview">Github <v-icon icon="mdi-github"></v-icon></a></p>
          </v-col>
        </v-row>
        <test-address :connector="evmConnector"></test-address>
        <v-divider></v-divider>
        <test-balance :connector="evmConnector"></test-balance>
        <v-divider></v-divider>
        <test-block-number :connector="evmConnector"></test-block-number>
        <v-divider></v-divider>
        <test-nonce :connector="evmConnector"></test-nonce>
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
