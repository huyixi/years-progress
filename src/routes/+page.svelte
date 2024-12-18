<script>
	import { onMount } from 'svelte';

	const MS_PER_SECOND = 1000;
	const MS_PER_MINUTE = MS_PER_SECOND * 60;
	const MS_PER_HOUR = MS_PER_MINUTE * 60;
	const MS_PER_DAY = MS_PER_HOUR * 24;

	let isRemainingTime = true;
	let currentYear = new Date().getFullYear();

	function getYearPercentage() {
		const now = new Date();
		const startOfYear = new Date(now.getFullYear(), 0, 1);
		const endOfYear = new Date(now.getFullYear() + 1, 0, 1);
		if (isRemainingTime) {
			return ((endOfYear - now) / (endOfYear - startOfYear)) * 100;
		} else {
			return ((now - startOfYear) / (endOfYear - startOfYear)) * 100;
		}
	}

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

	function getElapsedTime() {
		const now = new Date();
		const startOfYear = new Date(now.getFullYear(), 0, 1);
		const elapsed = now - startOfYear;

		const days = Math.floor(elapsed / MS_PER_DAY);
		const hours = Math.floor((elapsed % MS_PER_DAY) / MS_PER_HOUR);
		const minutes = Math.floor((elapsed % MS_PER_HOUR) / MS_PER_MINUTE);
		const seconds = Math.floor((elapsed % MS_PER_MINUTE) / MS_PER_SECOND);

		return { days, hours, minutes, seconds };
	}

	function handleTimeSwitch() {
		isRemainingTime = !isRemainingTime;
	}

	let currentTime = formatTime(new Date(), 'YYYY-MM-DD HH:mm:ss');
	let yearPercentage = getYearPercentage().toFixed(6);

	onMount(() => {
		const interval = setInterval(() => {
			yearPercentage = getYearPercentage().toFixed(6);
			currentTime = formatTime(new Date(), 'YYYY-MM-DD HH:mm:ss');
			timeDisplay = isRemainingTime ? getRemainingTime() : getElapsedTime();
		}, 250);

		return () => clearInterval(interval);
	});

	$: timeDisplay = isRemainingTime ? getRemainingTime() : getElapsedTime();
</script>

<div class="main">
	<h1>{currentYear} Progress</h1>
	<p>{currentTime}</p>
	<div class="progress-container" style="background: {isRemainingTime ? '#39d353' : 'red'}">
		<p class="progress-text">
			{yearPercentage}%
		</p>
		<div
			class="progress-bar"
			style="width: {100 - yearPercentage}%; margin-left: {isRemainingTime ? '0' : 'auto'};"
		></div>
	</div>
	<p>
		{timeDisplay.days} days,
		{timeDisplay.hours} hours,
		{timeDisplay.minutes} minutes,
		{timeDisplay.seconds} seconds
	</p>
	<button class="btn-switch" on:click={handleTimeSwitch}
		>Switch to {isRemainingTime ? 'Elapsed' : 'Remaining'} Time</button
	>
</div>

<style>
	.main {
		text-align: center;
		margin-top: max(40vh, 3rem);
	}

	.progress-container {
		position: relative;
		background: #39d353;
		border-radius: 10px;
		overflow: hidden;
		margin: 20px auto;
		height: 30px;
		width: 80%;
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
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
	}

	.btn-switch {
		display: inline-block;
		outline: 0;
		cursor: pointer;
		margin-top: 20px;
		padding: 5px 16px;
		font-size: 14px;
		font-weight: 500;
		line-height: 20px;
		vertical-align: middle;
		border: 1px solid;
		border-radius: 6px;
		color: #24292e;
		background-color: #fafbfc;
		border-color: #1b1f2326;
		box-shadow:
			rgba(27, 31, 35, 0.04) 0px 1px 0px 0px,
			rgba(255, 255, 255, 0.25) 0px 1px 0px 0px inset;
		transition: 0.2s cubic-bezier(0.3, 0, 0.5, 1);
		transition-property: color, background-color, border-color;
	}
</style>
