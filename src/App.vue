<script setup lang="ts">
import {onMounted, ref} from "vue";
import {IframeManager, EvmConnector} from '@noonewallet/widget-communicator'

let evmConnector = ref<EvmConnector | null>(null)

onMounted(async () => {
  const iframe = new IframeManager('noone-iframe', 'https://crypto-widget.noone.io/')
  await iframe.render()
  evmConnector.value = new EvmConnector(iframe)
})

const getAddress = async () => {
  if (!evmConnector.value) return console.error('EvmConnector is not initialized')
  const result = await evmConnector.value?.send({
    chainId: 1,
    method: 'getAddress'
  })
  console.log('result', result)
}
</script>

<template>
<v-container>
  <v-row no-gutters>
    <v-col cols="6">
      <v-sheet class="pa-2 ma-2">
        <button @click="getAddress">Get address</button>
      </v-sheet>
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
}
</style>
