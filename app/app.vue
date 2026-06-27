<script setup>
const { basicConfig } = useSysConfig()

useHead({
  titleTemplate: (title) => title ? `${title} - ${basicConfig.value.title}` : basicConfig.value.title,
})
</script>

<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>
