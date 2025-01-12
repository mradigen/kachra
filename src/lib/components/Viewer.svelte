<script lang="ts">
	import { env } from '$lib/env';
	import { onMount } from 'svelte';

	let text: string | null = null;

	onMount(async () => {
		const res = await fetch(`${env.SHORTENER_URL}/${slug}`);
		text = await res.text();
		if (text == 'Slug not found') {
			text = 'Paste not found';
		}
	});

	async function copyToClipboard(e: MouseEvent) {
		if (text) {
			try {
				await navigator.clipboard.writeText(text);

				let button = e.target as HTMLButtonElement;
				button.innerText = 'Copied';
				setTimeout(() => {
					button.innerText = 'Copy';
				}, 1000);
			} catch (err) {
				console.error('Failed to copy text:', err);
			}
		}
	}

	export let slug: string;
</script>

<pre
	class="w-full flex-1 resize-none overflow-scroll whitespace-pre-wrap bg-transparent font-[monospace] outline-none"
	spellcheck="false">{#if text != null}{text}{:else}Loading...{/if}
</pre>

<div class="absolute bottom-4 right-4 flex flex-col space-y-1">
	<button
		class="rounded bg-gray-500 px-4 py-2 text-white hover:bg-gray-600"
		on:click={copyToClipboard}
	>
		Copy
	</button>
</div>
