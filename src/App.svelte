<script>
	import { onMount } from 'svelte';

	export let times = 0;
	export let playing = false;
	let ls = null;

	const read = (itemName) => !!ls && (ls.getItem(itemName));
	const update = (itemName, itemValue) => !!ls && (ls.setItem(itemName, itemValue));

	function onEnded(e) {
		console.log("Ended");
		e.target.play();
		times++;
		update("spins", times);
		window.document.title = `Babushka: ${times} Spins!`;
	}

	function play(e) {
		document.getElementById("video").play();
	}

	function onPlay(e) {
		playing = true;
	}

	onMount(() => {
		typeof localStorage !== `undefined` && (ls = localStorage);
		times = read("spins") || 0;
	});
</script>

<main>
	<div class="video-container">
		<button on:click={play} class="{playing === true ? 'is-hidden' : ''}"></button>
		<video id="video" on:click={play} autoplay="autoplay" on:ended={onEnded} on:play={onPlay}>
			<source src="/media/babushka-spin.mp4" type="video/mp4" aria-label="A video of a grandma being spun by the head." />
		</video>
	</div>
	<h1>You have spun {times} times!</h1>
</main>

<style>
	main {
		text-align: center;
		padding: 0;
		max-width: 240px;
		margin: 0 auto;
	}

	.video-container {
		max-height: calc(100vh - 84px);
		height: 80vh;
		max-width: 100vw;
		position: relative;
	}

	video {
		width: auto;
		height: auto;
		max-height: 100%;
		max-width: 100%;
	}

	button {
		top: calc(50% - 37px);
		left: calc(50% - 37px);
		width: 74px;
		height: 74px;
		position: absolute;
		border-style: solid;
		box-sizing: border-box;
		border-width: 37px 0 37px 74px;
		border-color: transparent transparent transparent #fff;
		background-color: transparent;
		padding: 0;
		z-index: 500;
	}

	button.is-hidden {
		opacity: 0;
		display: none;
	}

	h1 {
		color: #fff;
		font-size: 2em;
		font-weight: 400;
		margin: 4px 0;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>