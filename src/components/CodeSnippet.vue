<script setup lang="ts">
import {onMounted, ref} from 'vue'
import hljs from 'highlight.js'
import javascript from 'highlight.js/lib/languages/javascript'
import 'highlight.js/styles/atom-one-light.css'

hljs.registerLanguage('javascript', javascript)

defineProps<{
  code: String
}>()

const codeEl = ref(null)

onMounted(() => {
  setTimeout(() => {
    hljs.highlightBlock(codeEl.value)
  }, 100)
})
</script>

<template>
<pre class="javascript">
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
