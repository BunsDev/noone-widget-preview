<script setup lang="ts">
import {computed, reactive, ref} from 'vue'
import TestTemplate from './TestTemplate.vue'
import {EvmConnector, type IErrorResponse} from '@noonewallet/widget-communicator'

const props = defineProps<{
  connector: EvmConnector
  chain: number
}>()

let error = ref<IErrorResponse | null>(null)
const data = reactive({
  result: ''
})

const code = computed(() => {
  return `const result = await props.connector.send({
    chainId: ${props.chain},
    method: 'getBlockNumber',
})`
})

const triggerEvent = async () => {
  if (!props.connector || !props.connector) return console.error('EvmConnector is not initialized')
  const result = await props.connector.send({
    chainId: props.chain,
    method: 'getBlockNumber'
  })
  if (result.success) {
    data.result = result?.data || ''
    error.value = null
  } else {
    error.value = result.error || null
    data.result = ''
  }
}
</script>

<template>
<test-template
  title="Get block number"
  :code="code"
  :error="error"
  :result="data.result"
  @trigger-event="triggerEvent"
>
</test-template>
</template>

<style scoped></style>
