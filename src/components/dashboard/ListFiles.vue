<template>
	<div class="flex items-center justify-center h-32" v-if="storage.fileLoading">
		<Loading class="w-20 h-20" />
	</div>
	<div
		class="flex items-center flex-col justify-center border-t pt-4"
		v-else-if="!storage.filteredFiles || storage.filteredFiles.length === 0"
	>
		<div class="font-medium text-lg pb-2 text-center">
			There is no file you have uploaded yet. <br />You can use the
			<strong>uploader</strong> below to upload it.
		</div>
		<Uploader />
	</div>
	<div class="space-y-1" v-else>
		<h4 class="font-medium text-xl">{{ title }}</h4>
		<div class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-5 xl:grid-cols-8 gap-2">
			<File v-for="file in storage.filteredFiles" :file="file" :key="file._id" />
		</div>
		<Pagination />
	</div>
</template>

<script setup>
import File from '@/components/dashboard/File.vue';
import { useFileStore } from '@/stores/file';
import Loading from '@/components/ui/Loading.vue';
import Uploader from '@/components/Uploader.vue';
import { useRouter } from 'vue-router';
import Pagination from '@/components/dashboard/Pagination.vue';
const storage = useFileStore();
const router = useRouter();

defineProps({
	title: {
		type: String,
		default: 'All Files',
	},
});
</script>
