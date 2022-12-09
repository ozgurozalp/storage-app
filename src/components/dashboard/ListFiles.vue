<template>
	<div class="flex items-center justify-center h-32" v-if="storage.fileLoading">
		<Loading class="w-20 h-20" />
	</div>
	<div
		class="flex items-center flex-col justify-center border-t pt-4"
		v-else-if="!storage.filteredFiles || storage.filteredFiles.length === 0"
	>
		<Alert class="rounded"> You have no files yet. You can add files below. </Alert>
		<Uploader />
	</div>
	<div class="space-y-1" v-else>
		<h4 class="font-medium text-xl">{{ title }}</h4>
		<div class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-5 xl:grid-cols-8 gap-2">
			<File v-for="file in storage.filteredFiles" :file="file" :key="file._id" />
		</div>
	</div>
	<div class="flex justify-center" v-if="storage.filesInfo?.totalPages > 1">
		<div class="flex flex-col items-center">
			<span class="text-sm text-gray-700 dark:text-gray-400">
				Showing
				<span class="font-semibold text-gray-900">{{
					storage.filesInfo?.currentPage
				}}</span>
				to
				<span class="font-semibold text-gray-900">{{
					storage.filesInfo?.pageSize
				}}</span>
				of
				<span class="font-semibold text-gray-900">{{
					storage.filesInfo?.count
				}}</span>
				Entries
			</span>
			<div class="inline-flex mt-2 xs:mt-0">
				<button
					:disabled="page === 1"
					@click="prev"
					class="inline-flex items-center px-4 py-2 text-sm font-medium text-white bg-primary rounded-l enabled:hover:bg-gray-900 disabled:opacity-80 disabled:cursor-not-allowed"
				>
					<svg
						aria-hidden="true"
						class="w-5 h-5 mr-2"
						fill="currentColor"
						viewBox="0 0 20 20"
						xmlns="http://www.w3.org/2000/svg"
					>
						<path
							fill-rule="evenodd"
							d="M7.707 14.707a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 1.414L5.414 9H17a1 1 0 110 2H5.414l2.293 2.293a1 1 0 010 1.414z"
							clip-rule="evenodd"
						/>
					</svg>
					Prev
				</button>
				<button
					:disabled="page === storage.filesInfo?.totalPages"
					@click="next"
					class="inline-flex items-center px-4 py-2 text-sm font-medium text-white bg-primary border-0 border-l border-gray-700 rounded-r enabled:hover:bg-gray-900 disabled:opacity-80 disabled:cursor-not-allowed"
				>
					Next
					<svg
						aria-hidden="true"
						class="w-5 h-5 ml-2"
						fill="currentColor"
						viewBox="0 0 20 20"
						xmlns="http://www.w3.org/2000/svg"
					>
						<path
							fill-rule="evenodd"
							d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z"
							clip-rule="evenodd"
						/>
					</svg>
				</button>
			</div>
		</div>
	</div>
</template>

<script setup>
import File from '@/components/dashboard/File.vue';
import { useFileStore } from '@/stores/file';
import Loading from '@/components/ui/Loading.vue';
import Uploader from '@/components/Uploader.vue';
import Alert from '@/components/ui/Alert.vue';
import { useRoute, useRouter } from 'vue-router';
import { ref, watch } from 'vue';
const storage = useFileStore();
const router = useRouter();
const route = useRoute();
const page = ref(route.query?.page ? parseInt(route.query.page) : 1);

defineProps({
	title: {
		type: String,
		default: 'All Files',
	},
});

watch([page, () => route.params.slug], () => {
	router.push({ query: { page: page.value } });
	storage.getFiles({
		page: page.value,
		tag: route.params.slug ?? null,
	});
});

function next() {
	page.value++;
}
function prev() {
	page.value--;
}
</script>
