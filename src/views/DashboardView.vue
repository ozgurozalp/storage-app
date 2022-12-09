<script setup>
import { useAuthStore } from '@/stores/auth';
import { useHead } from '@vueuse/head';
import { onMounted } from 'vue';
import SideBar from '@/components/dashboard/SideBar.vue';
import Main from '@/components/dashboard/Main.vue';
import { useFileStore } from '@/stores/file';
import { useRoute } from 'vue-router';

useHead({ title: 'Dashboard - Image Uploader' });
const auth = useAuthStore();
const storage = useFileStore();
const route = useRoute();

onMounted(() => {
	storage.getFolders();
	storage.getFiles({
		page: route.query.page ? parseInt(route.query.page) : 1,
		tag: route.params.slug ?? null,
	});
});
</script>

<template>
	<section
		class="grid grid-rows-[auto_1fr] lg:grid-rows-1 lg:grid-cols-[260px_1fr] h-full"
	>
		<SideBar />
		<Main />
	</section>
</template>
