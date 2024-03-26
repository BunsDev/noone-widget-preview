<script setup lang="ts">
import { onMounted } from 'vue'
import hljs from 'highlight.js'
import javascript from 'highlight.js/lib/languages/javascript'
hljs.registerLanguage('javascript', javascript)
import 'highlight.js/styles/atom-one-light.css'

defineProps<{
  code: String
}>()

onMounted(() => {
  setTimeout(() => {
    hljs.highlightBlock(document.querySelector('code'))
  }, 100)
})
</script>

<template>
  <pre class="javascript">
  <code>{{ code }}</code>
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
