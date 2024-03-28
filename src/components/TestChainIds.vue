<script setup lang="ts">
import {reactive, ref} from 'vue'
import TestTemplate from './TestTemplate.vue'
import { EvmConnector, type IErrorResponse } from '@noonewallet/widget-communicator'

const props = defineProps<{
  connector: EvmConnector
}>()
let error = ref<IErrorResponse | null>(null)

const data = reactive({
  result: '',
  error: '',
  title: 'Get EVM chains',
  code: `const result = await props.connector.send({
    chainId: 1,
    method: 'getChains'
})`
})

const triggerEvent = async () => {
  if (!props.connector || !props.connector) return console.error('EvmConnector is not initialized')
  const result = await props.connector.send({
    chainId: 1,
    method: 'getChains'
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
  :title="data.title"
  :code="data.code"
  :error="error"
  :result="data.result"
  @trigger-event="triggerEvent">
</test-template>
</template>
