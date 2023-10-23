<script>
	import { tweened } from 'svelte/motion';
	// @ts-ignore
	import ProgressBar from "@okrad/svelte-progressbar";

	// Timer
	let minutes = 1;
	let original = 10;
	let startTime = 0; 
	let timer = tweened(startTime)

	// Conditional rendering
	let startMenu = true;

	setInterval(() => {
			if ($timer > 0) $timer--;
		}, 1000);
		$: minutes = Math.floor($timer / 60);
		$: minname = minutes > 1 ? "mins" : "min";
		$: seconds = Math.floor($timer - minutes * 60)


	function startTimer(){
		startMenu = !startMenu
		timer = tweened(original)
	}

	function endTimer(){
		startMenu = !startMenu;
		clearTimeout;
	}

	
</script>

<!-- svelte-ignore non-top-level-reactive-declaration -->
<!-- svelte-ignore non-top-level-reactive-declaration -->
{#if startMenu}
<main class="container">
		<h1>Welcome to Pommodoro!</h1>
		<div class="row">
			<div class="button_style">
				<button on:click={() => startTimer()}>
					Start now!
				</button>
			</div>
		</div>
</main>
{:else}
<main>
	<div class="temporary">
		<button on:click={() => endTimer()}>
			Back
		</button>
	</div>
	<h1>Start studying!</h1>
	<div class="time">
	{#if $timer < 1}
			<p class="end_timer">END</p>
			<div class="button_style">
				<button on:click={() => timer = tweened(original)}>
					Restart
				</button>
			</div>
		{:else}
			<div class="progressBar">
				<ProgressBar 
					style='semicircle' 
					series={($timer/original)*100}
					thickness={2}
					thresholds={[
						{
							till: 20,       //Color stays red from 0% to 50%
							color: '#ff0000'
						},
						{
							till: 100,      //Color goes green from 51% to 100%
							color: '#ffc0cb'
						}
					]}
				/>
			</div>
			<p class="timeClock">
				<span>{minutes}:{seconds}</span>
			</p>
		{/if}
	</div>
</main>
{/if}