<script>
	import Modal from '../components/+Modal.svelte';
	import { goto } from '$app/navigation';
	import { quintOut } from 'svelte/easing';
	import { crossfade } from 'svelte/transition';
	import { flip } from 'svelte/animate';
	import foods from '../database/foods.json';
	import { wantedMixed, desserts } from "$lib/index.js"
	import { onMount } from 'svelte';

	let showModal = false;
	let isOpen = false;
    // Catagory 
	let all_mixed = ['หมู', 'จระเข้', 'ไก่', 'ทะเล', 'เป็ด', 'แป้ง', 'ของหวาน'];
    let all_countries = ['ไทย', 'จีน','ญี่ปุ่น','อิตาลี่']
	let mixed_filter = [];
    let countries_filter = '';
    let result = foods
	let desserts_ = foods
    const cal = () => {
        const mixed_filtered = foods.filter((item) =>
            item.mixed.some((mixed) => mixed_filter.includes(mixed))
        );
        const countries_filtered = mixed_filtered.filter(e => e == countries_filter)
        console.log(countries_filtered);
        result = mixed_filtered;
    }

	const calDessert = () => {
		const mixed_filtered = desserts_.filter((item) =>
            item.mixed.some((mixed) => mixed_filter.includes(mixed))
        );
        // const countries_filtered = mixed_filtered.filter(e => e == countries_filter)
        console.log(countries_filtered);
        desserts_ = mixed_filtered;
	}

	const closeDoorModal = () => {
		isOpen = !isOpen
	}
	$: console.log(result);
    $: wantedMixed.set(result)
	$: desserts.set(desserts_)
	const [send, receive] = crossfade({
		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;
			return {
				duration: 600,
				easing: quintOut,
				css: (t) => `
					transform: ${transform} scale(${tc});
					opacity: ${t}
				`
			};
		}
	});
</script>

<!-- Main Container in slot -->
<div
	class="flex relative items-center justify-center w-full h-screen max-h-screen overflow-hidden bg-black"
>
    <div class="w-full h-20 bg-black absolute z-30 top-0 black-drop"></div>
    <div class="w-full h-20 bg-black absolute z-30 bottom-0 black-up"></div>
    <div class="flex flex-col relative items-center py-20 z-50 w-full h-screen -top-12">
        <h1 class="text-9xl text-white font-bold title skew-x-6 drop-shadow-xl">MOHO HEW</h1>
        <div class="absolute z-20 text-white font-semibold bg-yellow-500 translate-y-[-110px] translate-x-[460px] p-2 aboutus w-full max-w-44 -skew-x-6">
			<p class="text-xs">SPONSORED BY.</p>
			<div class="grid grid-cols-3 grid-rows-2 gap-3">
				<img src="https://itcamp20.it.kmitl.ac.th/landing/itlogo.svg" alt="it" class='w-full h-full bg-black/30 p-1 rounded-md'>
				<img src="https://itcamp20.it.kmitl.ac.th/landing/sponsors/lactasoy.svg" alt="lactasoy" class='w-full h-full bg-black/30 p-1 rounded-md'>
				<img src="https://itcamp20.it.kmitl.ac.th/landing/sponsors/borntodev.svg" alt="borntodev" class='w-full h-full bg-black/30 p-1 rounded-md'>
				<img src="https://itcamp20.it.kmitl.ac.th/landing/sponsors/swf.svg" alt="swf" class='w-full h-full bg-black/30 p-1 rounded-md'>
				<img src="https://itcamp20.it.kmitl.ac.th/landing/sponsors/usefulfood.svg" alt="usefulfood" class='w-full h-full bg-black/30 p-1 rounded-md'>
				<img src="https://itcamp20.it.kmitl.ac.th/landing/sponsors/advice.svg" alt="advice" class='w-full h-full bg-black/30 p-1 rounded-md'>
			</div>
		</div>

    </div>

	<img
		src="background.png"
		alt="bg"
		class="absolute w-full h-screen inset-0 object-cover z-20 pointer-events-none"
	/>

	<button
		class="h-[20rem] w-[15rem] text-transparent absolute z-50 translate-x-[-200px]"
		on:click={() => {
			isOpen = !isOpen;
			setTimeout(() => {
				showModal = true
			}, 550);
		}}
	>
		Invisible
	</button>
	<!-- <button
		class="h-[20rem] w-[5rem] absolute z-50 translate-x-[-650px]"
	>
		Invisible
	</button> -->
	<img
		src="door.png"
		alt="door"
		class={`absolute w-full h-screen inset-0 object-cover z-10 transition-all duration-1000 scale-[1.01] translate-x-[3px] translate-y-[6px] ${isOpen ? '[transform:_perspective(100px)_rotateY(-90deg)]' : ''}`}
	/>
	<Modal bind:showModal state={() => closeDoorModal()}>
		<h2 slot="header">
			<div class="w-full flex justify-center items-center mb-10">
				<p>เลือกอาหาร</p>
			</div>
		</h2>

		<ol class="definition-list flex items-center justify-center gap-4">
			{#each all_mixed as wanted}
				<input
					type="checkbox"
					on:change={() => {
						if (!mixed_filter.find((e) => e == wanted)) {
							mixed_filter.push(wanted);
							mixed_filter = mixed_filter;
						} else {
							const index = mixed_filter.findIndex((e) => e == wanted);
							mixed_filter.splice(index, 1);
							mixed_filter = mixed_filter;
						}
						calDessert()
                        cal()
					}}
				/>
				{wanted}
			{/each}
		</ol>
	</Modal>
</div>


<style>

    .title {
        position: relative;
        animation: downtitle 3s;
    }

    @keyframes downtitle {
        0%   {top: 1000px;}
        100% { top: 0px }
    }

    .aboutus {
        position: relative;
        animation: dropper 5s;
    }
    
    @keyframes dropper {
        0% { bottom: 100px; opacity: 0;}
        25% { bottom: 70px; opacity: 0;}
        50% { bottom: 10px;}
        100% { bottom: 0px; }
    }

   .black-drop {
        position: absolute;
        animation: blackdrop 3s;
    }

    .black-up {
        position: absolute;
        animation: blackup 3s;
    }

    @keyframes blackdrop {
        0% { top: -100px}
        100% { top: 0px; }
    }
    
    @keyframes blackup {
        0% { bottom: -100px}
        100% { bottom: 0px; }
    }
</style>
