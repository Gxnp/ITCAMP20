<script>
	import { onMount } from 'svelte';
import '../app.pcss';
	let loading = true
	onMount(() => {
		setTimeout(() => {
			loading = false
		}, 300);
	})
</script>

<!-- <div>adgasdg</div> -->
{#if loading}
	<div class="flex flex-col h-screen w-full items-center justify-center overflow-hidden">
		<img  src="https://itcamp20.it.kmitl.ac.th/landing/logoNew.webp" class="animate-spin" />
	</div>
{:else}
<div class="font-[Prompt] flex flex-col">
	<slot></slot>
</div>
{/if}

<style>
	
</style>