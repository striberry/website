<script lang="ts">
	/* eslint-disable svelte/no-navigation-without-resolve */
	import { onMount } from 'svelte';
	import { photos } from '$lib/data/background';

	let currentIndex = 0;
	const duration = 30000; // ms

	function getRandomIndex(excludeIndex: number) {
		let idx;
		do {
			idx = Math.floor(Math.random() * photos.length);
		} while (idx === excludeIndex && photos.length > 1);
		return idx;
	}

	function showNext() {
		const nextIndex = getRandomIndex(currentIndex);
		document.body.style.backgroundImage = `url('${photos[nextIndex].imageUrl}')`;
		currentIndex = nextIndex;
	}

	onMount(() => {
		currentIndex = getRandomIndex(-1);

		document.body.style.backgroundSize = 'cover';
		document.body.style.backgroundPosition = 'center';
		document.body.style.backgroundRepeat = 'no-repeat';
		document.body.style.backgroundImage = `url('${photos[currentIndex].imageUrl}')`;

		const interval = setInterval(showNext, duration);
		return () => clearInterval(interval);
	});
</script>

<div class="bg-credit">
	<a
		class="work-name"
		target="_blank"
		rel="noopener noreferrer"
		href={photos[currentIndex].workUrl}
	>
		Photo
	</a>,
	<a
		class="artist-name"
		target="_blank"
		rel="noopener noreferrer"
		href={photos[currentIndex].artistUrl}
	>
		{photos[currentIndex].artist}
	</a>,
	<a
		class="source-name"
		target="_blank"
		rel="noopener noreferrer"
		href={photos[currentIndex].sourceUrl}
	>
		{photos[currentIndex].source}
	</a>
</div>