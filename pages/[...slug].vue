<script setup lang="ts">
const route = useRoute();
const { data: page } = await useAsyncData(route.path, () =>
  queryContent(route.path).findOne(),
);

if (!page.value) {
  throw createError({
    statusCode: 404,
    statusMessage: "Page not found",
    fatal: true,
  });
}

useSeoMeta({
  title: page.value.title,
  ogTitle: `${page.value.title} – Kirby SEO Audit`,
  description: page.value.description,
  ogDescription: page.value.description,
});

defineOgImageComponent("Default", {
  title: page.value.title,
  description: page.value.description,
});
</script>

<template>
  <UContainer :ui="{ constrained: 'max-w-screen-md' }">
    <UPage v-if="page">
      <UPageHeader :title="page.title" :links="page.links" />

      <UPageBody prose>
        <ContentRenderer v-if="page.body" :value="page" />
      </UPageBody>
    </UPage>
  </UContainer>
</template>
