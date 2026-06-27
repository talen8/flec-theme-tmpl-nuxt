<script setup>
const route = useRoute()
const { data: article } = useArticle(route.params.slug)

useHead({
  title: computed(() => article.value?.title),
})
</script>

<template>
  <article>
    <h1>{{ article?.title }}</h1>
    <div v-html="renderMarkdown(article?.content || '')" />
  </article>
</template>
