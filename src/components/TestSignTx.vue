<script setup lang="ts">
import {reactive, ref, computed} from 'vue'
import TestTemplate from './TestTemplate.vue'
import {EvmConnector, type IErrorResponse} from '@noonewallet/widget-communicator'

const props = defineProps<{
  connector: EvmConnector
}>()

let error = ref<IErrorResponse | null>(null)
const tx = ref(`{
  "to": "0xca8a66887dfbef870a2d96de536986516a37fa12",
  "value": "0xad78ebc5ac6200000",
  "gasPrice": "0x2678011db",
  "gas": "0x5208",
  "nonce": "0x0"
}`)
const data = reactive({
  result: ''
})

const code = computed(() => {
  return `const result = await props.connector.send({
    chainId: 1,
    method: 'signTransaction',
    params: '${tx.value}'
})`
})
const triggerEvent = async () => {
  if (!props.connector || !props.connector) return console.error('EvmConnector is not initialized')
  const fixedStr = tx.value.replace(/'/g, '"').replace(/(\w+):/g, '"$1":');
  const processedTx = JSON.parse(fixedStr)
  const result = await props.connector.send({
    chainId: 1,
    method: 'signTransaction',
    params: processedTx
  })
  if (result.success) {
    data.result = result.data || ''
    error.value = null
  } else {
    error.value = result.error || null
    data.result = ''
  }
}
</script>

<template>
<test-template
  title="Sign transaction"
  :code="code"
  :error="error"
  :result="data.result"
  @trigger-event="triggerEvent"
>
  <template v-slot:fields>
    <v-textarea v-model="tx" label="Transaction"></v-textarea>
  </template>
</test-template>
</template>
