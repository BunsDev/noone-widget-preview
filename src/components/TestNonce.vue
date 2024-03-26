<script setup lang="ts">
import {reactive} from 'vue'
import TestTemplate from './TestTemplate.vue'

const props = defineProps<{
  connector: EvmConnector
}>()

const data = reactive({
  result: '',
  error: '',
  code: `const result = await props.connector.send({
    chainId: 1,
    method: 'getNonce'
})`
})

const triggerEvent = async () => {
  if (!props.connector || !props.connector) return console.error('EvmConnector is not initialized')
  const result = await props.connector.send({
    chainId: 1,
    method: 'getNonce'
  })
  console.log('result', result)
  if (result.success) {
    data.result = result.data.toString()
    data.error = ''
  } else {
    data.error = result.error
    data.result = ''
  }
}
</script>

<template>
<test-template
  title="Get nonce"
  :code="data.code"
  :error="data.error"
  :result="data.result"
  @trigger-event="triggerEvent"
>
</test-template>
</template>
