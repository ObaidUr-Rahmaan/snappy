<script lang="ts" setup>
import { useLinks } from "~~/composables/useLinks";

useHead({
  title: "Dashboard",
});
definePageMeta({
  middleware: "auth",
});

const { data, refresh } = await useLinks().getAllLinks();

const handleRefresh = (newLink: any) => {
  refresh();
};
</script>

<template>
  <section class="container mx-auto pt-28">
    <h1 class="text-2xl font-bold text-white">Dashboard</h1>
    <LinkForm @shorten="handleRefresh" />
    <div class="mt-10 links-wrapper">
      <LinkCard v-for="link in data" :key="link.id" :link="link"> </LinkCard>
    </div>
  </section>
</template>

export { };
