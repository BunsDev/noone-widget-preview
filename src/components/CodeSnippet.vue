<script setup lang="ts">
import {onMounted, ref, watch} from 'vue'
import hljs from 'highlight.js'
import javascript from 'highlight.js/lib/languages/javascript'
import 'highlight.js/styles/atom-one-light.css'

hljs.registerLanguage('javascript', javascript)

const props = defineProps<{
  code: String
}>()

const show = ref(true)
const codeEl = ref(null)

watch(() => props.code, () => {
  show.value = false
  setTimeout(() => {
    show.value = true
  }, 100)
  setTimeout(() => {
    hljs.highlightBlock(codeEl.value)
  }, 200)
})
onMounted(() => {
  setTimeout(() => {
    if (codeEl.value) {
      hljs.highlightBlock(codeEl.value)
    }
  }, 200)
})
</script>

<template>
<pre class="javascript" v-if="show">
  <code ref="codeEl">{{ code }}</code>
</pre>
</template>

<style scoped>
pre {
  font-size: 0;
}

code {
  font-size: 14px;
}

</style>
