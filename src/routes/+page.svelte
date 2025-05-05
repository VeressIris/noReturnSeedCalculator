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
		<h3 class="text-white">{dailyRunSeed}</h3>
	</div>
	<div class="center mt-2">
		<h3>Pick a date:</h3>
		<input
			type="date"
			bind:value={selectedDate}
			class="border-2 border-gray-300 rounded-lg py-2 px-2.5 my-2"
		/>
		{#if selectedDate}
			<p>
				The seed for {new Date(selectedDate).toLocaleDateString('en-US', dateOptions)} was:
			</p>
			<p class="text-white">{getSeed(new Date(selectedDate))}</p>
		{/if}
	</div>
</div>
