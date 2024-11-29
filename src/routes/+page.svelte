<script>
	import { onMount } from 'svelte';

	let currentTime = new Date().toLocaleString();

	function getCurrentYearProgress() {
		const now = new Date();
		const currentYear = now.getFullYear();
		const startOfYearStamp = new Date(currentYear, 0, 1).getTime();
		const endOfYearStamp = new Date(currentYear + 1, 0, 1).getTime();
		const elapsed = now.getTime() - startOfYearStamp;
		return (elapsed / (endOfYearStamp - startOfYearStamp)) * 100;
	}

	let progress = getCurrentYearProgress();

	onMount(() => {
		const interval = setInterval(() => {
			currentTime = new Date().toLocaleString();
			progress = getCurrentYearProgress();
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});
</script>

<div>
	<h1>2024 Progress</h1>
	<p>Current Time: {currentTime}</p>
	<div class="progress-container">
		<div class="progress-bar" style="width: {progress}%">
			<p class="progress-info">
				{progress}%
			</p>
		</div>
	</div>
</div>

<style>
	.progress-container {
		width: 300px;
		height: 30px;
		background-color: #e0e0e0;
		border-radius: 15px;
		overflow: hidden;
	}

	.progress-bar {
		height: 100%;
		background-color: #4caf50;
		transition: width 0.5s ease;
	}

	.progress-info {
		line-height: 30px;
		text-align: center;
		color: #fff;
	}
</style>
