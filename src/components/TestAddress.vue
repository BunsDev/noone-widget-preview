<script setup lang="ts">
import { computed, reactive, ref } from 'vue'
import { EvmConnector } from '@noonewallet/widget-communicator'
import CodeSnippet from './CodeSnippet.vue'

const props = defineProps<{
  connector: EvmConnector
}>()
const data = reactive({
  address: '',
  error: ''
})
const codeVisible = ref(false)
const code = ref(`const result = await props.connector.send({
    chainId: 1,
    method: 'getAddress'
})`)
const computedBtnTitle = computed(() => {
  return codeVisible.value ? 'Hide code' : 'Show code'
})

const getAddress = async () => {
  if (!props.connector || !props.connector) return console.error('EvmConnector is not initialized')
  const result = await props.connector.send({
    chainId: 1,
    method: 'getAddress'
  })
  if (result.success) {
    data.address = result.data
    data.error = ''
  } else {
    data.error = result.error
    data.address = ''
  }
}

const showCode = () => {
  codeVisible.value = !codeVisible.value
}
</script>

<template>
  <v-container>
    <v-row>
      <v-col cols="4">
        <v-btn variant="outlined" size="small" @click="getAddress"> Get address </v-btn>
      </v-col>
      <v-col cols="4">
        <v-btn variant="outlined" size="small" prepend-icon="mdi-code-tags" @click="showCode">
          {{ computedBtnTitle }}
        </v-btn>
      </v-col>
      <v-col v-show="codeVisible">
        <code-snippet :code="code"></code-snippet>
      </v-col>
      <v-col v-if="data.address">
        <span v-if="data.address"> <b>Result:</b> {{ data.address }} </span>
        <span v-if="data.error"> <b>Error:</b> {{ data.error }} </span>
      </v-col>
    </v-row>
  </v-container>
</template>

<style scoped></style>
