<script setup lang="ts">
const nuxtApp = useNuxtApp();
const { activeHeadings, updateHeadings } = useScrollspy();

const links = computed(() => [
	{
		label: "学习篇",
		icon: "i-heroicons-book-open",
		to: "/study",
	},
	{
		label: "生活篇",
		to: "/life",
	},
]);

nuxtApp.hooks.hookOnce("page:finish", () => {
	updateHeadings([
		document.querySelector("#features"),
		document.querySelector("#pricing"),
		document.querySelector("#testimonials"),
	]);
});
</script>

<template>
  <UHeader :links="links">
    <template #logo>
      <span class="text-xl font-bold">2024 年度总结</span>
      <UBadge
        label="2024"
        variant="subtle"
        class="mb-0.5"
      />
    </template>
   
    <template #right>
      <UColorModeSelect/>
    </template>
  </UHeader>
</template>
