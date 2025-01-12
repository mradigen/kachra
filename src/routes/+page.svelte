<script lang="ts">
	import Editor from '$lib/components/Editor.svelte';
	import Github from '$lib/components/Github.svelte';
	import Viewer from '$lib/components/Viewer.svelte';
	import { onMount } from 'svelte';

	let slug: string | null = null;

	onMount(() => {
		slug = Array.from(new URLSearchParams(window.location.search).keys())[0]; // Gets slug from "k.phy0.in?SLUG"
	});
</script>

<Github></Github>

<div
	class="flex h-screen w-screen flex-col space-y-8 overflow-hidden bg-gray-800 p-4 font-[monospace] text-lg text-gray-300 lg:px-[25%]"
>
	{#if slug != null}
		<Viewer {slug}></Viewer>
	{:else}
		<Editor></Editor>
	{/if}
</div>
