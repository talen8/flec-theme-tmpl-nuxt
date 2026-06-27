<script setup>
const { data } = useArticleList()
const articles = computed(() => data.value?.list || [])
</script>

<template>
  <div>
    <article v-for="article in articles" :key="article.id">
      <NuxtLink :to="article.url">
        <h2>{{ article.title }}</h2>
        <p>{{ article.excerpt }}</p>
      </NuxtLink>
    </article>
    <p v-if="!articles.length">暂无文章</p>
  </div>
</template>
