<script>
	import { onMount } from 'svelte';

	// Constants for time calculations
	const MS_PER_SECOND = 1000;
	const MS_PER_MINUTE = MS_PER_SECOND * 60;
	const MS_PER_HOUR = MS_PER_MINUTE * 60;
	const MS_PER_DAY = MS_PER_HOUR * 24;

	// Get current year progress as a percentage
	function getYearPercentage() {
		const now = new Date();
		const startOfYear = new Date(now.getFullYear(), 0, 1);
		const endOfYear = new Date(now.getFullYear() + 1, 0, 1);
		return ((now - startOfYear) / (endOfYear - startOfYear)) * 100;
	}

	function formatDate(date, format) {
		const padZero = (num) => (num < 10 ? `0${num}` : num);

		const parts = {
			YYYY: date.getFullYear(),
			MM: padZero(date.getMonth() + 1),
			DD: padZero(date.getDate()),
			HH: padZero(date.getHours()),
			mm: padZero(date.getMinutes()),
			ss: padZero(date.getSeconds())
		};

		return format.replace(/YYYY|MM|DD|HH|mm|ss/g, (match) => parts[match]);
	}

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

<<<<<<< HEAD
	function formatTime(date, format) {
		const padZero = (num) => (num < 10 ? `0${num}` : num);

		const parts = {
			YYYY: date.getFullYear(),
			MM: padZero(date.getMonth() + 1),
			DD: padZero(date.getDate()),
			HH: padZero(date.getHours()),
			mm: padZero(date.getMinutes()),
			ss: padZero(date.getSeconds())
		};

		return format.replace(/YYYY|MM|DD|HH|mm|ss/g, (match) => parts[match]);
	}

	let currentTime = formatTime(new Date(), 'YYYY-MM-DD HH:mm:ss');
=======
	let currentTime = formatDate(new Date(), 'YYYY-MM-DD HH:mm:ss');
>>>>>>> 8fef8eb686183dd65eb13ade85908c7eb83b081a
	let progress = getYearPercentage().toFixed(6);
	let remainingTime = getRemainingTime();

	onMount(() => {
		const interval = setInterval(() => {
<<<<<<< HEAD
			progress = getYearPercentage().toFixed(6);
			currentTime = formatTime(new Date(), 'YYYY-MM-DD HH:mm:ss');
=======
			currentTime = formatDate(new Date(), 'YYYY-MM-DD HH:mm:ss');
			progress = getYearPercentage().toFixed(6);
>>>>>>> 8fef8eb686183dd65eb13ade85908c7eb83b081a
			remainingTime = getRemainingTime();
		}, 250);

		return () => clearInterval(interval);
	});
</script>

<div class="main">
	<h1>2024 Progress</h1>
	<p>{currentTime}</p>

	<div class="progress-container">
		<p class="progress-text">
			{progress}%
		</p>
		<div class="progress-bar" style="width: {progress}%;"></div>
	</div>

	<p>
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
		margin: 32vh auto;
		width: 80%;
	}

	.progress-container {
		position: relative;
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

	.progress-text {
		position: absolute;
		z-index: 10;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
	}

	p {
		color: #555;
	}
</style>
