<script setup lang="ts">
import {computed, ref, defineEmits} from 'vue'
import CodeSnippet from './CodeSnippet.vue'
import {type IErrorResponse} from '@noonewallet/widget-communicator'
defineProps<{
  title: string
  code: string
  result?: string
  error?: IErrorResponse | null
}>()
const emit = defineEmits(['trigger-event'])
const codeVisible = ref(false)

const computedBtnTitle = computed(() => {
  return codeVisible.value ? 'Hide code' : 'Show code'
})
const triggerEvent = () => {
  emit('trigger-event')
}
const showCode = () => {
  codeVisible.value = !codeVisible.value
}
</script>

<template>
<v-container class="my-4 pa-0">
  <h2 class="my-4">{{ title }}</h2>
  <v-row align="center">
    <v-col cols="5">
      <v-btn :style="{width: '100%'}" variant="outlined" size="small" @click="triggerEvent">{{ title }}</v-btn>
    </v-col>
    <v-col cols="4">
      <v-btn variant="tonal" size="small" prepend-icon="mdi-code-tags" @click="showCode">
        {{ computedBtnTitle }}
      </v-btn>
    </v-col>
    <v-col v-show="codeVisible" cols="12">
      <code-snippet :code="code"></code-snippet>
    </v-col>
    <v-col cols="12" v-if="result">
      <b>Result: </b><span> {{ result }}</span>
    </v-col>
    <v-col cols="12" v-if="error">
      <b class="text-red">Error: </b><span> {{ error }}</span>
    </v-col>
  </v-row>
</v-container>
</template>

<style scoped></style>

