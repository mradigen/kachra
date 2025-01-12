<script>
	import { onMount } from 'svelte';
	import hljs from 'highlight.js';
	import 'highlight.js/styles/github-dark.css';
	import { uploadFile } from '$lib/minio';
	import { Expiry } from '$lib/expiry';
	import { shortenURL } from '$lib/short';
	import { env } from '$lib/env';

	let code = '';
	let selectedExpiry = env.DEFAULT_EXPIRY;

	const saveCode = async () => {
		const blob = new Blob([code] /*, { type: mimeType }*/);
		const file = new File([blob], 'kachra.txt' /*, { type: mimeType }*/);

		await uploadFile(file, Expiry.day);

		const longURL = await uploadFile(file, selectedExpiry);
		if (longURL == null) return alert('Error');
		const shortURL = await shortenURL(longURL);
		if (shortURL == null) return alert('Error');

		window.location.search = shortURL;
	};

	onMount(() => {
		hljs.highlightAll();
	});
</script>

<!-- svelte-ignore a11y_autofocus -->
<textarea
	bind:value={code}
	class="w-full flex-1 resize-none bg-transparent outline-none"
	placeholder="Drop your kachra here..."
	autofocus
	autocomplete="off"
	autocapitalize="none"
	spellcheck="false"
></textarea>

<div class="absolute bottom-4 right-4 flex flex-col space-y-1">
	<div class="w-fit">
		<select class="rounded-md bg-gray-600 px-3 py-2" bind:value={selectedExpiry}>
			{#each Object.entries(Expiry) as [key, value]}
				<option {value}>{key.charAt(0).toUpperCase() + key.slice(1)}</option>
			{/each}
		</select>
	</div>
	<button class="rounded bg-gray-500 px-4 py-2 text-white hover:bg-gray-600" on:click={saveCode}>
		Save
	</button>
</div>
