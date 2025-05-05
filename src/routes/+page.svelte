<script lang="ts">
	const releaseDate = new Date('01/19/2024');
	const firstSeed = 19741;
	const dailyRunSeed: number = getSeed(new Date());

	let selectedDate: Date | null = null;

	const dateOptions = {
		year: 'numeric',
		month: 'long',
		day: 'numeric'
	};

	function daysSince(date1: Date, date2: Date): number {
		const timeDiff = date1.getTime() - date2.getTime();
		return Math.floor(timeDiff / (1000 * 60 * 60 * 24));
	}

	function getSeed(date: Date): number {
		return daysSince(date, releaseDate) + firstSeed;
	}
</script>

<div class="flex flex-col items-center justify-center my-8 mx-20">
	<h1>Welcome to No Return Seed Calculator</h1>
	<h2>Today's daily run seed is:</h2>
	<p>{dailyRunSeed}</p>
	<h3>Pick a date:</h3>
	<p>See the seed of any daily run</p>
	<input type="date" bind:value={selectedDate} class="border-2 border-gray-300 rounded-md p-2" />
	{#if selectedDate}
		<p class="mt-4">
			The seed for {new Date(selectedDate).toLocaleDateString('en-US', dateOptions)} was:
		</p>
		<p>{getSeed(new Date(selectedDate))}</p>
	{/if}
</div>
