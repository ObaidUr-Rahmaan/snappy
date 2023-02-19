<script lang="ts" setup>
useHead({
  title: "Snappy - Nuxt 3 + Supabase URL Shortener with analytics",
});
</script>

<template>
  <section class="grid h-screen place-items-center">
    <div class="max-w-3xl mx-auto text-center">
      <h1 class="text-5xl font-bold heading lg:text-6xl">
        Shorten your links with Snappy - the blazingly fast URL shortener.
      </h1>
      <p class="w-10/12 mx-auto my-10 text-lg md:w-2/3">
        Share your content effortlessly with sleek and memorable links.
      </p>
      <NuxtLink :to="{ name: 'dashboard' }" class="text-base btn"
        >Try it Now</NuxtLink
      >
    </div>
  </section>
</template>

<style>
.heading {
  line-height: 110%;
  background: linear-gradient(
    180deg,
    #ffffff 0%,
    rgba(255, 255, 255, 0) 188.28%
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  /* text-fill-color: transparent; */
}
</style>
