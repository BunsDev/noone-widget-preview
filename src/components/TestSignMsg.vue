<script setup lang="ts">
import {reactive, ref, computed} from 'vue'
import TestTemplate from './TestTemplate.vue'
import {EvmConnector, type IErrorResponse} from '@noonewallet/widget-communicator'

const props = defineProps<{
  connector: EvmConnector
}>()

let error = ref<IErrorResponse | null>(null)
const message = ref('')
const data = reactive({
  result: ''
})

const code = computed(() => {
  return `const result = await props.connector.send({
    chainId: 1,
    method: 'signMessage',
    params: '${message.value}'
})`
})
const triggerEvent = async () => {
  if (!props.connector || !props.connector) return console.error('EvmConnector is not initialized')
  const result = await props.connector.send({
    chainId: 1,
    method: 'signMessage',
    params: message.value
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
  title="Sign message"
  :code="code"
  :error="error"
  :result="data.result"
  @trigger-event="triggerEvent"
>
  <template v-slot:fields>
    <v-text-field
      v-model="message"
      label="Message"
    ></v-text-field>
  </template>
</test-template>
</template>
