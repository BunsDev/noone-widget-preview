<script setup lang="ts">
import {computed, defineEmits, reactive} from 'vue'

const props = defineProps<{
  chains: Array<number>
}>()
const selected = reactive({
  title: '1',
  subtitle: 'Ethereum',
})
const emit = defineEmits(['select-chain'])

const itemProps = computed(() => {
  return props.chains.map((chain) => {
    return {
      title: chain.chainId,
      subtitle: chain.name
    }
  })
})
const selectChain = (chain) => {
  selected.title = chain
  emit('select-chain', chain)
}
</script>

<template>
<v-container class="my-4 pa-0">
  <v-row align="center">
    <v-col cols="6">
      <v-select
        v-model="selected"
        density="compact"
        label="Chain"
        :item-props="true"
        :items="itemProps"
        @update:model-value="selectChain"
      ></v-select>
    </v-col>
  </v-row>
</v-container>
</template>
