<script>
	import { tick } from 'svelte';
	import Tool from './Tool.svelte';

	let flipResult = '';
	let coinVisible = true;
	let coin;

	async function flip() {
		flipResult = '';
		coinVisible = false;

		await tick();

		coinVisible = true;
		const randomFlip = Math.random();

		if (randomFlip <= 0.5) {
			flipResult = 'heads';
		} else {
			flipResult = 'tails';
		}
	}
</script>

<Tool title="Coin Flip" size="small">
	{#if coinVisible}
		<div bind:this={coin} on:click={flip} id="coin" class={flipResult}></div>
	{/if}
</Tool>

<style lang="scss">
	#coin {
		margin: 0 auto;
		width: 150px;
		height: 150px;
		cursor: pointer;
		background: url(/coin_sheet.png) center top;
	}

	#coin.heads {
		animation: flipCoin 0.25s steps(18) 5 forwards;
	}

	#coin.tails {
		animation: flipCoin 0.25s steps(18) 5.5 forwards;
	}

	@keyframes flipCoin {
		from {
			background-position-y: 0 * 150px;
		}
		to {
			background-position-y: 18 * 150px;
		}
	}
</style>