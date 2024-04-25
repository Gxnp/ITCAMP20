<script>
	export let showModal; // boolean
	import { goto } from '$app/navigation';
    export let state = {};
	let dialog; // HTMLDialogElement

	$: if (dialog && showModal) dialog.showModal();
</script>

<!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
<dialog
	class="max-w-xl rounded-lg border-none p-28"
	bind:this={dialog}
	on:close={() => (showModal = false)}
	on:click|self={() => dialog.close()}
>
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div on:click|stopPropagation class="container flex flex-col backdrop-blur-md">
		<slot name="header" />
		<hr />
		<slot />
		<hr />
		<!-- svelte-ignore a11y-autofocus -->
		<!-- {#if (window.location.pathname != '/random')}
			<button autofocus on:click={() => {goto('random')}}>เข้าไปด้านใน</button>
		{/if} -->
			<button class="bg-emerald-400 p-2 rounded-md text-white font-bold" autofocus on:click={() => {goto('random')}}>เข้าไปด้านใน</button>
			<button class="mt-5 flex items-center justify-center bg-red-400 p-2 rounded-md text-white font-bold" on:click={() => {
				dialog.close()
				state()
			}}>ปิด Pop up</button>
	</div>
</dialog>

<style>
	dialog {
		max-width: 52rem;
		border-radius: 0.5em;
		border: none;
		padding: 5px;
	}
	dialog::backdrop {
		background: rgba(0, 0, 0, 0.3);
	}
	dialog > div {
		padding: 1em;
	}
	dialog[open] {
		animation: zoom 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	@keyframes zoom {
		from {
			transform: scale(0.95);
		}
		to {
			transform: scale(1);
		}
	}
	dialog[open]::backdrop {
		animation: fade 0.2s ease-out;
	}
	@keyframes fade {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}
	button {
		display: block;
	}
</style>
