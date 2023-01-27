<script>
	import { AccordionItem, Accordion } from 'flowbite-svelte';

	import { getStorage, ref, getMetadata, listAll } from 'firebase/storage';
	import { onMount } from 'svelte';

	import FileTable from './FileTable.svelte';
	import UploadRow from './UploadRow.svelte';

	const storage = getStorage();
	const refs = ['work', 'clients', 'pictures', 'misc'];
	$: files = {
		work: [],
		clients: [],
		pictures: [],
		misc: []
	};
	function getFileData() {
		files = {
			work: [],
			clients: [],
			pictures: [],
			misc: []
		};
		const user = localStorage.getItem('uid');
		refs.forEach((folder) => {
			let reference = ref(storage, `${user}/${folder}`);
			listAll(reference)
				.then((res) => {
					res.items.forEach((itemRef) => {
						getMetadata(itemRef).then((metaData) => {
							files[folder] = [...files[folder], metaData];
						});
					});
				})
				.catch((error) => {});
		});
	}
	onMount(() => {
		let user = '';
		getFileData();
	});
</script>

<Accordion>
	<AccordionItem>
		<span slot="header">Work Files</span>
		<div slot="arrowup">
			<svg
				class="w-6 h-6 shrink-0 rotate-180"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"
				/></svg
			>
		</div>
		<span slot="arrowdown">
			<svg
				class="w-6 h-6 shrink-0"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"
				/></svg
			>
		</span>
		<UploadRow folder="work" functionProp={() => getFileData()} />
		{#if files.work.length > 0}
			<FileTable data={files.work} folder="work" functionProp={() => getFileData()} />
		{/if}
	</AccordionItem>
	<AccordionItem>
		<span slot="header">Client File</span>
		<div slot="arrowup">
			<svg
				class="w-6 h-6 shrink-0 rotate-180"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"
				/></svg
			>
		</div>
		<span slot="arrowdown">
			<svg
				class="w-6 h-6 shrink-0"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"
				/></svg
			>
		</span>
		<p class="mb-2 text-gray-500 dark:text-gray-400">
			Lorem ipsum dolor sit amet, consectetur adipisicing elit. Illo ab necessitatibus sint
			explicabo ...
		</p>
	</AccordionItem>
	<AccordionItem>
		<span slot="header">Pictures</span>
		<div slot="arrowup">
			<svg
				class="w-6 h-6 shrink-0 rotate-180"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"
				/></svg
			>
		</div>
		<span slot="arrowdown">
			<svg
				class="w-6 h-6 shrink-0"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"
				/></svg
			>
		</span>
		<p class="mb-2 text-gray-500 dark:text-gray-400">
			Lorem ipsum dolor sit amet, consectetur adipisicing elit. Illo ab necessitatibus sint
			explicabo ...
		</p>
	</AccordionItem>
	<AccordionItem>
		<span slot="header">Misc Files</span>
		<div slot="arrowup">
			<svg
				class="w-6 h-6 shrink-0 rotate-180"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"
				/></svg
			>
		</div>
		<span slot="arrowdown">
			<svg
				class="w-6 h-6 shrink-0"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"
				/></svg
			>
		</span>
		<p class="mb-2 text-gray-500 dark:text-gray-400">
			Lorem ipsum dolor sit amet, consectetur adipisicing elit. Illo ab necessitatibus sint
			explicabo ...
		</p>
	</AccordionItem>
</Accordion>
