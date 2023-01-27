<script>
	import { Button } from 'flowbite-svelte';

	import { getStorage, ref, getDownloadURL, deleteObject } from 'firebase/storage';
	function bytesToSize(bytes) {
		const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB'];
		if (bytes === 0) return 'n/a';
		const i = parseInt(Math.floor(Math.log(bytes) / Math.log(1024)), 10);
		if (i === 0) return `${bytes} ${sizes[i]})`;
		return `${(bytes / 1024 ** i).toFixed(1)} ${sizes[i]}`;
	}
	const storage = getStorage();
	function downloadFile(folder, name) {
		const uid = localStorage.getItem('uid');
		let path = `${uid}/${folder}/${name}`;
		getDownloadURL(ref(storage, path))
			.then((url) => {
				const link = document.createElement('a');
				link.href = url;
				link.click();
			})
			.catch((error) => {
				console.error(error);
			});
	}
	function deleteFile(folder, name) {
		const uid = localStorage.getItem('uid');
		let path = `${uid}/${folder}/${name}`;
		const fileRef = ref(storage, path);
		// Delete the file
		window.confirm('Permanently delete this file?');
		deleteObject(fileRef)
			.then(() => {
				functionProp();
			})
			.catch((error) => {
				// Uh-oh, an error occurred!
			});
	}
	export let data;
	export let folder;
	export let functionProp = () => {};
</script>

<table class="w-screen px-10">
	<thead>
		<tr>
			<th class="w-1/4">File Name</th>
			<th class="w-1/4">Uploaded</th>
			<th class="w-1/4">File Size</th>
			<th class="w-1/4">Actions</th>
		</tr>
	</thead>
	<tbody>
		{#each data as { name, timeCreated, size }}
			<tr>
				<th scope="row">{name}</th>
				<td class="text-center">{timeCreated.substring(0, 10)}</td>
				<td class="text-center">{bytesToSize(size)}</td>
				<td class="text-center">
					<button
						type="button"
						class="inline-block mx-2 my-1 px-6 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out"
						on:click={() => downloadFile(folder, name)}>Download</button
					>
					<button
						type="button"
						class="inline-block mx-2 my-1 px-6 py-2.5 bg-red-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-red-700 hover:shadow-lg focus:bg-red-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-red-800 active:shadow-lg transition duration-150 ease-in-out"
						on:click={() => deleteFile(folder, name)}>Delete</button
					>
				</td>
			</tr>
		{/each}
	</tbody>
</table>
