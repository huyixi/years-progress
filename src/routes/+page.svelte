<script>
	import { onMount } from 'svelte';

	let currentTime = new Date().toLocaleString();

	function getCurrentYearProgress() {
		const now = new Date();
		const currentYear = now.getFullYear();
		const startOfYearStamp = new Date(currentYear, 0, 1).getTime();
		const endOfYearStamp = new Date(currentYear + 1, 0, 1).getTime();
		const elapsed = now.getTime() - startOfYearStamp;
		return ((elapsed / (endOfYearStamp - startOfYearStamp)) * 100).toFixed(6);
	}

	function getRemainingTime() {
		const now = new Date();
		const currentYear = now.getFullYear();
		const endOfYearStamp = new Date(currentYear + 1, 0, 1).getTime();
		const remaining = endOfYearStamp - now.getTime();
		const remainingDay = Math.floor(remaining / (1000 * 60 * 60 * 24));
		const remainingHour = Math.floor((remaining % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
		const remainingMinute = Math.floor((remaining % (1000 * 60 * 60)) / (1000 * 60));
		const remainingSecond = Math.floor((remaining % (1000 * 60)) / 1000);

		return {
			day: remainingDay,
			hour: remainingHour,
			minute: remainingMinute,
			second: remainingSecond
		};
	}

	let progress = getCurrentYearProgress();
	let remainingTime = getRemainingTime();

	onMount(() => {
		const interval = setInterval(() => {
			currentTime = new Date().toLocaleString();
			progress = getCurrentYearProgress();
			remainingTime = getRemainingTime();
		}, 250);

		return () => {
			clearInterval(interval);
		};
	});
</script>

<div class="main">
	<h1>2024 Progress</h1>
	<p>Current Time: {currentTime}</p>
	<div class="progress-container">
		<div class="progress-bar" style="width: {progress}%">
			<p class="progress-info">
				{progress}%
			</p>
		</div>
	</div>
	<p>
		Remaining:

		{#if remainingTime.day > 0}
			{remainingTime.day}
		{:else}
			0
		{/if}days,

		{#if remainingTime.hour > 0}
			{remainingTime.hour}
		{:else}
			0
		{/if}hours,

		{#if remainingTime.minute > 0}
			{remainingTime.minute}
		{:else}
			0
		{/if}minutes,

		{#if remainingTime.second > 0}
			{remainingTime.second}
		{:else}
			0
		{/if}seconds
	</p>
</div>

<style>
	.main {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 90vh;
	}

	.progress-container {
		width: 300px;
		height: 30px;
		background-color: #4caf50;
		border-radius: 16px;
		overflow: hidden;
	}

	.progress-bar {
		height: 100%;
		background-color: #e0e0e0;
		transition: width 0.5s ease;
	}

	.progress-info {
		line-height: 30px;
		text-align: center;
		color: #000;
	}
</style>
