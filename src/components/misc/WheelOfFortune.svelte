<script context="module">
	const fontFamily = '-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji"';
	const fontSize = 22;
	const fontWeight = '500';

	function easeOut(t, b, c, d) {
		if ((t /= d / 2) < 1) return (c / 2) * t * t + b;
		return (-c / 2) * (--t * (t - 2) - 1) + b;
	}
</script>

<script>
	import { onMount } from 'svelte';

	export let width = 600;
	export let height = 600;
	export let speed = 1000;
	export let duration = 3000;
	export let prizes = [];
	export let primaryColor = '#57b121';
	export let secondaryColor = '#8be058';
	export let spinButtonHidden = false;
	export let onComplete = () => {
	};

	let canvas = null;
	let ctx = null;
	let animateId = null;

	let rotateTime = 0;
	let rotateAllTime = 0;
	let rotateChange = 0;
	let startRotatingAt = 0;
	let isRotating = false;
	let awardRotate = 0;
	let centerX = 0;
	let centerY = 0;
	let R = 0;
	let TEXT_R = 0;
	let INSERT_R = 0;

	function getSelectedPrize() {
		let startAngle = (startRotatingAt * 180) / Math.PI,
			awardAngle = (awardRotate * 180) / Math.PI,
			pointerAngle = 90,
			overAngle = (startAngle + pointerAngle) % 360,
			restAngle = 360 - overAngle,
			index = Math.floor(restAngle / awardAngle);

		return prizes[index];
	}

	function noticePrize() {
		const prize = getSelectedPrize();
		onComplete(prize);
	}

	function fittingString(c, str, maxWidth, fontWeight, fontFamily, maxSize) {
		let width = 0;
		let wantedSize = 0;

		for (let size = maxSize; size > 11; size--) {
			c.font = `${fontWeight} ${size}px ${fontFamily}`;
			wantedSize = size - 2;
			width = c.measureText(str).width;

			if (width <= maxWidth) {
				c.font = `${fontWeight} ${wantedSize}px ${fontFamily}`;
				return str;
			}
		}

		let len = str.length;
		let ellipsis = '…';
		let ellipsisWidth = c.measureText(ellipsis).width;

		while (width >= maxWidth - ellipsisWidth && len-- > 0) {
			str = str.substring(0, len);
			width = c.measureText(str).width;
		}

		c.font = `${fontWeight} ${wantedSize}px ${fontFamily}`;
		return str + ellipsis;
	}

	function drawTurntable() {
		ctx.clearRect(0, 0, canvas.width, canvas.height);

		if (canvas.width === 0 || canvas.height === 0) return;

		for (let [i, prize] of prizes.entries()) {
			const _currentStartRotate = startRotatingAt + awardRotate * i;
			const _currentEndRotate = _currentStartRotate + awardRotate;

			ctx.save();

			i % 2 === 0
				? (ctx.fillStyle = primaryColor)
				: (ctx.fillStyle = secondaryColor);

			ctx.lineWidth = 1;
			ctx.strokeStyle = 'black';

			ctx.beginPath();
			ctx.arc(
				centerX,
				centerY,
				R,
				_currentStartRotate,
				_currentEndRotate,
				false,
			);
			ctx.arc(
				centerX,
				centerY,
				INSERT_R,
				_currentEndRotate,
				_currentStartRotate,
				true,
			);
			ctx.fill();
			ctx.closePath();
			ctx.restore();

			ctx.save();
			ctx.beginPath();
			//ctx.font = `bold 14px -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji"`;
			ctx.fillStyle = '#fff';
			ctx.textBaseline = 'middle';

			const currentX =
				Math.cos(_currentStartRotate + awardRotate / 2) * TEXT_R;
			const currentY =
				Math.sin(_currentStartRotate + awardRotate / 2) * TEXT_R;

			ctx.translate(centerX + currentX, centerY + currentY);
			ctx.rotate(_currentStartRotate + awardRotate / 2 + Math.PI / 2);

			const maxWidth = TEXT_R - 45;
			const fittingText = fittingString(
				ctx,
				prize,
				maxWidth,
				fontWeight,
				fontFamily,
				fontSize,
			);

			// Font vertical
			ctx.rotate((90 / 180) * Math.PI);

			ctx.fillText(fittingText, 0, 0);

			ctx.closePath();
			ctx.restore();
		}
	}

	function rotateTurntable() {
		rotateTime += 20;

		if (rotateTime >= rotateAllTime) {
			isRotating = false;
			noticePrize();
			return;
		}

		let _rotateChange = (rotateChange - easeOut(rotateTime, 0, rotateChange, rotateAllTime)) * (Math.PI / 180);

		startRotatingAt += _rotateChange;
		drawTurntable();

		animateId = requestAnimationFrame(rotateTurntable);
	}

	function handleSpin() {
		if (isRotating) return;

		isRotating = true;
		rotateTime = 0;
		rotateAllTime = Math.random() * 5 + duration;
		rotateChange = Math.random() * 10 + speed / 100;

		rotateTurntable();
	}

	function stopSpin() {
		isRotating = false;
		cancelAnimationFrame(animateId);
		noticePrize();
	}

	onMount(() => {
		// Init
		ctx = canvas.getContext('2d');
		canvas.width = width;
		canvas.height = height;
		canvas.style.width = width;
		canvas.style.height = height;

		awardRotate = (Math.PI * 2) / prizes.length;

		centerX = canvas.width / 2;
		centerY = canvas.height / 2;
		R = canvas.width / 2 - 20;
		TEXT_R = R - 10;
		INSERT_R = 0;

		drawTurntable();


		return () => {
			// Destroy context
			cancelAnimationFrame(animateId);
			canvas = null;
			ctx = null;
		};
	});
</script>

<div class="wheel wheel-section">
	<canvas class="wheel-section-canvas" bind:this={canvas}></canvas>

	{#if !spinButtonHidden}
		<div class="wheel-section-btn" on:click={handleSpin}>
			SPIN
		</div>
	{/if}
</div>

<style lang="scss">
	.wheel-section {
		$click-btn-color: #31c27c;
		$btn-width: 80px;
		$btn-height: $btn-width;

		position: relative;

		&::before, &::after {
			clear: both;
			content: "";
			display: table;
			zoom: 1;
		}

		&-canvas {
			display: block;
			pointer-events: none;
			margin: auto;
		}

		&-btn {
			width: $btn-width;
			height: $btn-height;
			line-height: $btn-height;
			border-radius: 50%;
			box-shadow: 0 0 5px rgba(0, 0, 0, .1);
			border: 2px solid #fff;
			color: #fff;
			background: $click-btn-color;
			text-align: center;
			font-size: 16px;
			font-weight: bold;
			position: absolute;
			left: 0;
			right: 0;
			top: 0;
			bottom: 0;
			margin: auto;
			cursor: pointer;
			z-index: 2;
			user-select: none;

			&::before {
				content: "";
				position: absolute;
				border: 20px solid transparent;
				border-bottom-color: $click-btn-color;
				left: 50%;
				margin-left: -20px;
				top: -30px;
				z-index: 1;
			}
		}
	}
</style>