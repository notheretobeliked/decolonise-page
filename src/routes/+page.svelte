<script lang="ts">
	import { inview } from 'svelte-inview'

	let showCredits = false
	let divs: HTMLElement[] = []
	const options = {
		threshold: 0.5 // Experiment with this value, e.g., 0.1, 0.3, 0.5, etc.
	}
	let isInView: boolean

	type Item = {
		text: string
		caption: string
		color: string
	}

	const items: Item[] = [
		{
			text: 'Sunset over the serene blue ocean',
			caption:
				'The sun dips below the horizon, painting the sky in shades of orange and pink. The calm sea reflects the vibrant colors, creating a mesmerizing view.',
			color: 'bg-red-500'
		},
		{
			text: 'Exploring the depths of the forest',
			caption:
				'Underneath the dense canopy, the air is fresh and cool. The path winds through ancient trees, revealing the untamed beauty of nature.',
			color: 'bg-green-500'
		},
		{
			text: 'Gazing at the star-filled night sky',
			caption:
				"Away from the city's lights, the stars shine brightly. The Milky Way stretches across the sky, a glittering band in the vast universe.",
			color: 'bg-indigo-500'
		},
		{
			text: 'Climbing the challenging mountain trail',
			caption:
				'Each step takes you higher, revealing new vistas. The summit looms ahead, promising a breathtaking view from the top.',
			color: 'bg-yellow-500'
		},
		{
			text: 'Walking through the vibrant flower field',
			caption:
				'The field is a riot of colors, with flowers of every hue. The gentle breeze carries the sweet fragrance, creating a sense of peace.',
			color: 'bg-pink-500'
		}
	]

	function handleInViewChange(index: number, inView: boolean): void {
		if (inView) {
			const newUrlHash = `#section-${index}`
			if (window.location.hash !== newUrlHash) {
				window.history.pushState({}, '', newUrlHash)
			}
		}
	}
</script>

<div class="flex flex-row">
	<div class="snap-y snap-mandatory w-full h-screen overflow-y-auto">
		{#each items as item, index}
			<div
				use:inview={options}
				on:inview_enter={(event) => {
					const { inView, entry, scrollDirection, observer, node } = event.detail
					handleInViewChange(index, inView)
				}}
				bind:this={divs[index]}
				id={`section-${index}`}
				class={`snap-center ${item.color} w-full h-full flex flex-row items-center justify-center text-center`}
			>
				<div class="w-full flex flex-col items-center">
					<div class="relative pb-[30%] w-1/2">
						<svg
							class="absolute top-0 bottom-0 right-0 left-0"
							width="100%"
							height="100%"
							viewBox="0 0 100 100"
							preserveAspectRatio="none"
							xmlns="http://www.w3.org/2000/svg"
						>
							<!-- Background rectangle -->
							<rect width="100%" height="100%" fill="white" />

							<!-- Two lines intersecting in the middle to form an X -->
							<line x1="0" y1="0" x2="100" y2="100" stroke="black" stroke-width="0.2" />
							<line x1="100" y1="0" x2="0" y2="100" stroke="black" stroke-width="0.2" />
						</svg>
					</div>
					<h2>{item.text}</h2>
				</div>
				<button
					class={`transition-all h-full bg-white ${showCredits ? 'w-96' : 'w-24'}`}
					on:click={() => (showCredits = !showCredits)}
					aria-label="Toggle Credits"
				>
					{#if showCredits}
						<p>{item.caption}</p>
					{:else}
						show credits
					{/if}
				</button>
			</div>
		{/each}
	</div>
</div>
