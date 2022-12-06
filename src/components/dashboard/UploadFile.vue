<template>
	<Button @click="openModal" variant="primary" class="px-6">Upload File</Button>
	<Teleport to="body">
		<Modal title="Upload Folder" :open="modalStatus" :close="closeModal">
			<Uploader />
		</Modal>
	</Teleport>
</template>

<script setup>
import { ref, watch } from 'vue';
import Modal from '@/components/ui/Modal.vue';
import Button from '@/components/ui/Button.vue';
import { useFileStore } from '@/stores/file';
import { useToast } from 'vue-toastification';
import Uploader from '@/components/Uploader.vue';

const input = ref(null);
const folderName = ref('');
const creatingFolder = ref(false);
const storage = useFileStore();
const toast = useToast();

const modalStatus = ref(false);
const closeModal = () => {
	modalStatus.value = false;
};
watch(input, () => {
	input.value?.focus();
});

const openModal = () => {
	modalStatus.value = true;
};

const folderHandler = async () => {
	if (storage.folders?.some(folder => folder.name === folderName.value)) {
		toast.error('Folder name already exists, Please try another name');
		return;
	}
	creatingFolder.value = true;
	await storage.createFolder(folderName.value);
	creatingFolder.value = false;
	folderName.value = '';
	closeModal();
};
</script>
