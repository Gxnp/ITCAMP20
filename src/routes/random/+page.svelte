<script>
	import foods from '../../database/foods.json';
	import Modal from '../../components/+Modal.svelte';
	import { wantedMixed, desserts } from '$lib/index.js';
	import { goto } from '$app/navigation';
	let in_wantedMixed;
	let showModal = false;
	let showDessert = false

	$: console.log($wantedMixed);
	in_wantedMixed = $wantedMixed;
	console.log('This is in_wantedMixed: ', in_wantedMixed.length);

	const final_random = in_wantedMixed[Math.floor(Math.random() * in_wantedMixed.length)];
	const desserts_ = $desserts[Math.floor(Math.random() * in_wantedMixed.length)]
	// คัดกรอง
	$: filter_place = foods.filter((food) => food.mixed != in_wantedMixed);
	// คัดกรองสถานที่
</script>

<!-- <img
		src="background.png"
		alt="bg"
		class="absolute w-full h-screen inset-0 object-cover z-20 pointer-events-none"
	/> -->

<div class="w-full h-screen flex justify-center items-center">
	<img
		src="/background-2.png"
		alt="background of page 2"
		class="w-full h-full absolute -z-20 overflow-noscroll"
	/>
	<div class="flex">
		<div class="flex flex-col items-center justify-center group">
			<button on:click={() => (showModal = !showModal)} class="relative z-40">
				<img src="/box.png" alt="Box1" class="mt-48 cursor-pointer drop-shadow-lg z-50" />
			</button>
			<img
				src="/light.png"
				alt="light"
				class="absolute mb-60 opacity-0 pointer-events-none transition-all duration-300 group-hover:opacity-100 z-20 hidden group-hover:block"
			/>
		</div>
		<div class="flex flex-col items-center justify-center group">
			<button on:click={() => (showModal = !showModal)} class="relative z-40">
				<img src="/box.png" alt="Box1" class="mt-48 cursor-pointer drop-shadow-lg z-50" />
			</button>
			<img
				src="/light.png"
				alt="light"
				class="absolute mb-72 opacity-0 pointer-events-none transition-all duration-300 group-hover:opacity-100 z-20 hidden group-hover:block"
			/>
		</div>
	</div>
	<Modal bind:showModal>
		<h1 slot="header">
			<div class="w-full flex justify-center items-center mb-3">
				<p>กินอะไรดีจั้ฟ</p>
			</div>
		</h1>

		<h1 class="flex justify-center items-center mb-5 mt-5">{final_random.title}</h1>
		<img src={final_random.img} alt="food" />
	</Modal>

	<Modal bind:showDessert>
		<h1 slot="header">
			<div class="w-full flex justify-center items-center mb-3">
				<p>กินอะไรดีจั้ฟ</p>
			</div>
		</h1>

		<h1 class="flex justify-center items-center mb-5 mt-5">{desserts_.title}</h1>
		<img src={desserts_.img} alt="food" />
	</Modal>
</div>
