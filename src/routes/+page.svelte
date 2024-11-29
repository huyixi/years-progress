<script>
	import { onMount } from 'svelte';

	// Constants for time calculations
	const MS_PER_SECOND = 1000;
	const MS_PER_MINUTE = MS_PER_SECOND * 60;
	const MS_PER_HOUR = MS_PER_MINUTE * 60;
	const MS_PER_DAY = MS_PER_HOUR * 24;

	// Get current year progress as a percentage
	function getYearProgress() {
		const now = new Date();
		const startOfYear = new Date(now.getFullYear(), 0, 1);
		const endOfYear = new Date(now.getFullYear() + 1, 0, 1);
		return ((now - startOfYear) / (endOfYear - startOfYear)) * 100;
	}

	// Get remaining time until the end of the year
	function getRemainingTime() {
		const now = new Date();
		const endOfYear = new Date(now.getFullYear() + 1, 0, 1);
		const remaining = endOfYear - now;

		const days = Math.floor(remaining / MS_PER_DAY);
		const hours = Math.floor((remaining % MS_PER_DAY) / MS_PER_HOUR);
		const minutes = Math.floor((remaining % MS_PER_HOUR) / MS_PER_MINUTE);
		const seconds = Math.floor((remaining % MS_PER_MINUTE) / MS_PER_SECOND);

		return { days, hours, minutes, seconds };
	}

	let currentTime = new Date().toLocaleString();
	let progress = getYearProgress().toFixed(6);
	let remainingTime = getRemainingTime();

	// Update every 250ms
	onMount(() => {
		const interval = setInterval(() => {
			currentTime = new Date().toLocaleString();
			progress = getYearProgress().toFixed(6);
			remainingTime = getRemainingTime();
		}, 250);

		return () => clearInterval(interval);
	});
</script>

<div class="main">
	<h1>2024 Progress</h1>
	<p>Current Time: {currentTime}</p>

	<div class="progress-container">
		<div class="progress-bar" style="width: {progress}%;">
			{progress}%
		</div>
	</div>

	<p>
		Remaining:
		{remainingTime.days} days,
		{remainingTime.hours} hours,
		{remainingTime.minutes} minutes,
		{remainingTime.seconds} seconds
	</p>
</div>

<style>
	.main {
		font-family: Arial, sans-serif;
		text-align: center;
		margin: 36vh auto;
		width: 80%;
	}

	.progress-container {
		background: #39d353;
		border-radius: 10px;
		overflow: hidden;
		margin: 20px 0;
		height: 30px;
	}

	.progress-bar {
		height: 100%;
		background: #f0f0f0;
		text-align: center;
		line-height: 30px;
		color: #000;
		font-size: 14px;
	}

	p {
		color: #555;
	}
</style>
