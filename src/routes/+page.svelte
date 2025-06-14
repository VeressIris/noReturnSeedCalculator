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
	<div class="center mt-2">
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
</div>
