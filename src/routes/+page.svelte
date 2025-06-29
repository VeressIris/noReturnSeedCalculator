<script lang="ts">
	import { render } from 'svelte/server';

	const releaseDate = new Date('2024-01-19');
	// const firstSeed = 19741; // by my (poor?) calculations
	const dateOptions = {
		year: 'numeric',
		month: 'long',
		day: 'numeric'
	};

	const characterRotation = [
		'Abby',
		'Dina',
		'Joel',
		'Lev',
		'Tommy',
		'Yara',
		'Jesse',
		'Manny',
		'Mel',
		'Marlene',
		'Bill',
		'Ellie'
	];

	const dailyRunSeed: number = getSeed(new Date());
	const dailyRunCharacter: string = getCharacter(new Date());

	let selectedDate: Date | null = null;

	function daysSince(date1: Date, date2: Date): number {
		const timeDiff = date1.getTime() - date2.getTime();
		return Math.floor(timeDiff / (1000 * 60 * 60 * 24));
	}

	// my reverse engineered way
	// function getSeedOld(date: Date): number {
	// 	return daysSince(date, releaseDate) + firstSeed;
	// }

	// the literal way they calculate the seed
	function getSeed(date: Date): number {
		return daysSince(date, new Date('1970-01-01'));
	}

	function getCharacter(date: Date): string {
		const characterIndex = daysSince(date, releaseDate) % characterRotation.length;
		return characterRotation[characterIndex];
	}
</script>

{#snippet otherDays(selectedDate: Date, tense: String)}
	<p class="mt-2">
		The seed for <span class="font-medium text-orange-500">
			{new Date(selectedDate).toLocaleDateString('en-US', dateOptions)}</span
		>
		{tense}:
	</p>
	<p class="font-semibold">{getSeed(new Date(selectedDate))}</p>
	<p>
		playing as <span class="text-orange-500 font-semibold"
			>{getCharacter(new Date(selectedDate))}</span
		>
	</p>
{/snippet}

<svelte:head><title>Daily Run Seed | No return - TLOU2</title></svelte:head>

<div class="center mt-14 mx-12">
	<div class="mb-4">
		<h1 class="mb-2">
			Welcome to <br /> <span class="text-orange-500 uppercase">No Return</span> Seed Calculator
		</h1>
		<p>See the seed of any daily run.</p>
	</div>
	<div class="center my-6">
		<h2 class="mb-2 text-center">
			<span class="text-orange-500">Today</span>'s daily run seed is:
		</h2>
		<h3>{dailyRunSeed}</h3>
		<p class="special-paragraph">
			playing as <span class="text-orange-500 font-semibold">{dailyRunCharacter}</span>
		</p>
	</div>
	<div class="center my-2">
		<h3>Pick a date:</h3>
		<input
			type="date"
			bind:value={selectedDate}
			class="border-2 border-gray-300 rounded-lg py-2 px-2.5 my-2"
		/>
		{#if selectedDate}
			{#if new Date(selectedDate) <= new Date()}
				{@render otherDays(selectedDate, 'was')}
			{:else}
				{@render otherDays(selectedDate, 'will be')}
			{/if}
		{/if}
	</div>
	<div class="mt-24">
		<h2 class="mb-2">Mentions</h2>
		<div class="flex flex-col items-center my-4">
			<iframe
				class="w-[384px] h-[216px] sm:w-[640px] sm:h-[360px]"
				src="https://www.youtube.com/embed/NHqPko5n_v0"
				title="No Return Daily Run Seed Calculator"
				frameborder="0"
				allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
				referrerpolicy="strict-origin-when-cross-origin"
				allowfullscreen
			></iframe>
			<h3 class="mt-2">Anthony Caliber</h3>
		</div>
		<div class="flex flex-col items-center my-4">
			<blockquote
				class="reddit-embed-bq"
				style="height:316px"
				data-embed-theme="dark"
				data-embed-height="316"
			>
				<a
					href="https://www.reddit.com/r/thelastofus/comments/1kx6957/where_do_streamers_find_the_no_return_seed_code/"
					>Where do streamers find the No Return seed code to play a particular Daily Run after it's
					no longer the active Daily Run?</a
				><br /> by<a href="https://www.reddit.com/user/Lincolns_Revenge/">u/Lincolns_Revenge</a>
				in<a href="https://www.reddit.com/r/thelastofus/">thelastofus</a>
			</blockquote>
			<script src="https://embed.reddit.com/widgets.js" charset="UTF-8"></script>
			<h3 class="mt-2">Reddit thread</h3>
		</div>
	</div>
</div>
