<script lang="ts">
	import { smoothload } from '$lib/actions.js';
	import { media } from '$lib/api';
	import Carousel from '$lib/components/Carousel.svelte';
	import { fade, blur, slide, scale, draw } from 'svelte/transition';

	export let data;

	$: images = data.featured.images;
	$: backdrop = images.backdrops.find((image) => !image.iso_639_1) || images.backdrops[0];
	$: logo = images.logos.find((image) => image.iso_639_1 === 'en');
</script>

<svelte:head>
	<title>SvelteFlix</title>
</svelte:head>

<div in:fade class="">
	<!-- <h1>Today's top movies</h1> -->

	<a href="/movies/{data.featured.id}">
		<img
			class="backdrop md:h-[50lvh] object-cover"
			alt={data.featured.title}
			src={media(backdrop.file_path, 1280)}
			style="aspect-ratio: {backdrop.aspect_ratio}"
			use:smoothload
		/>

		{#if logo}
			<div class="absolute h-full w-full flex items-end pb-10 justify-center">
				<img
					class="w-1/2 md:w-1/3 drop-shadow"
					alt={data.featured.title}
					src={media(logo.file_path, 500)}
					style="aspect-ratio: {logo.aspect_ratio}"
					use:smoothload
				/>
			</div>
		{/if}
	</a>
</div>

<div class="px-4">
	<Carousel title="Trending" href="/movies/trending" movies={data.trending} />
	<Carousel title="Now playing" href="/movies/now_playing" movies={data.now_playing} />
	<Carousel title="Upcoming" href="/movies/upcoming" movies={data.upcoming} />
</div>

<style>
	a {
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	.backdrop {
		width: 100%;
		border-radius: 5px;
		-webkit-mask-image: linear-gradient(to bottom, black 0%, black 80%, transparent 100%);
		mask-image: linear-gradient(to bottom, black 0%, black 80%, transparent 100%);
	}
</style>
