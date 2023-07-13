<script>
	import { spring } from 'svelte/motion';
	import Table from './table.svelte';
	import image from './fish-PhotoRoom.png';

	let array = [
		{ cityName: 'Seattle', minCustomer: 23, maxCustomer: 65, avgCookieSaleHour: 6.3 },
		{ cityName: 'Tokyo', minCustomer: 3, maxCustomer: 24, avgCookieSaleHour: 1.2 },
		{ cityName: 'Dubai', minCustomer: 11, maxCustomer: 38, avgCookieSaleHour: 3.7 },
		{ cityName: 'Paris', minCustomer: 20, maxCustomer: 38, avgCookieSaleHour: 6.3 },
		{ cityName: 'Lima', minCustomer: 2, maxCustomer: 65, avgCookieSaleHour: 6.3 }
	];

	let cityName = '';
	let minCustomer = 0;
	let maxCustomer = 0;
	let avgCookieSaleHour = 0;

	$: minCustomer = Math.min(minCustomer, maxCustomer);

	function handleSubmit(e) {
		const existingCityIndex = array.findIndex((city) => city.cityName === cityName);

		if (existingCityIndex !== -1) {
			array[existingCityIndex] = { cityName, minCustomer, maxCustomer, avgCookieSaleHour };
		} else {
			array = [...array, { cityName, minCustomer, maxCustomer, avgCookieSaleHour }];
		}
	}

	let coords = spring(
		{ x: 50, y: 50 },
		{
			stiffness: 0.05,
			damping: 0.2
		}
	);

	let size = spring(10);

	let svgContainer;

	function handleMouseMove(e) {
		const containerRect = svgContainer.getBoundingClientRect();
		const x = e.clientX - containerRect.left;
		const y = e.clientY - containerRect.top;
		coords.set({ x, y });
	}
	function handleScroll(event) {
		if (event.target === document.activeElement) {
			event.target.value = parseFloat(event.target.value) + event.deltaY * 0.01;
		}
	}
</script>

<body role="application" on:mousemove={handleMouseMove}>
	<div class="svg-container" bind:this={svgContainer}>
		<div
			role="button"
			tabindex="0"
			on:mousedown={() => size.set(30)}
			on:mouseup={() => size.set(10)}
		>
			<svg>
				<!-- <circle cx={$coords.x} cy={$coords.y} r={$size} /> -->
				<image
					href={image}
					x={$coords.x - $size}
					y={$coords.y - $size}
					width={10 * $size}
					height={10 * $size}
				/>
			</svg>
		</div>
	</div>

	<form on:submit|preventDefault={handleSubmit}>
		<label for="cityName">City Name:</label>
		<input type="text" id="cityName" bind:value={cityName} placeholder="City Name" />

		<label for="avgCookieSaleHour">Average sales per hour:</label>
		<input
			type="number"
			id="avgCookieSaleHour"
			min="0"
			step="0.1"
			bind:value={avgCookieSaleHour}
			on:wheel={handleScroll}
			placeholder="Average sales per hour"
		/>

		<label for="maxCustomer">Max customers per hour:</label>
		<input
			type="number"
			id="maxCustomer"
			min="0"
			step="0.1"
			bind:value={maxCustomer}
			on:wheel={handleScroll}
			placeholder="Max customers per hour"
		/>

		<label for="minCustomer">Minimum customers per hour:</label>
		<input
			type="number"
			id="minCustomer"
			min="0"
			step="0.1"
			bind:value={minCustomer}
			on:wheel={handleScroll}
			placeholder="Minimum customers per hour"
		/>

		<button disabled={!(minCustomer && maxCustomer && avgCookieSaleHour)} type="submit">
			Submit
		</button>
	</form>

	<div class="tableContainer">
		<Table cityObjArrInput={array} />
	</div>
	<div class="ocean">
		<div class="wave" />
		<div class="wave" />
	</div>
</body>

<style>


	svg {
		position: absolute;
		width: 100%;
		height: 100%;
	}

	/* CREDIT https://codepen.io/tedmcdo/pen/PqxKXg */

	body {
		height: 100%;
		overflow-x: hidden;
	}
	body {
		background: radial-gradient(
			ellipse at center,
			rgba(255, 254, 234, 1) 0%,
			rgba(255, 254, 234, 1) 35%,
			#b7e8eb 100%
		);
		/* overflow: hidden; */
	}

	.ocean {
		height: 5%;
		width: 100%;
		position: absolute;
		bottom: 0;
		left: 0;
		background: #015871;
		animation: heightWave 7s ease-in-out infinite;
	}

	.wave {
		background: url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/85486/wave.svg) repeat-x;
		position: absolute;
		top: -198px;
		width: 6400px;
		height: 198px;
		animation: wave 7s cubic-bezier(0.36, 0.45, 0.63, 0.53) infinite;
		transform: translate3d(0, 0, 0);
	}

	.wave:nth-of-type(2) {
		top: -175px;
		animation: wave 7s cubic-bezier(0.36, 0.45, 0.63, 0.53) -0.125s infinite,
			swell 7s ease -1.25s infinite;
		opacity: 1;
	}

	@keyframes heightWave {
		0% {
			height: 5%;
		}
		50% {
			height: 15%;
		}
		100% {
			height: 5%;
		}
	}

	@keyframes wave {
		0% {
			margin-left: 0;
		}
		100% {
			margin-left: -1600px;
		}
	}

	@keyframes swell {
		0%,
		100% {
			transform: translate3d(0, -25px, 0);
		}
		50% {
			transform: translate3d(0, 5px, 0);
		}
	}
	form {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-bottom: 20px;
		border: 1px solid #ccc;
		padding: 20px;
		border-radius: 4px;
		box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
	}

	input[type='number'] {
		width: 200px;
		padding: 10px;
		margin: 5px;
		border: 1px solid #ccc;
		border-radius: 4px;
		text-align: center;
	}

	button[type='submit'] {
		padding: 10px 20px;
		background-color: #4caf50;
		color: white;
		border: none;
		border-radius: 4px;
		cursor: pointer;
		font-size: 16px;
		transition: background-color 0.3s ease;
		margin-bottom: 10px;
	}

	button[type='submit']:disabled {
		background-color: #999;
		cursor: not-allowed;
	}

	button[type='submit']:hover {
		background-color: #45a049;
	}

	.svg-container {
		position: absolute;
		width: 100%;
		height: 100%;
		left: 0;
		top: 0;
		z-index: 0;
		pointer-events: none;
	}

	/* Additional styles */
	button[type='submit']:not([disabled]) {
		position: relative;
		overflow: hidden;
		z-index: 1;
	}

	button[type='submit']:not([disabled]):before {
		content: '';
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 0;
		height: 0;
		border-radius: 50%;
		border: 2px solid #fff;
		opacity: 0;
		z-index: -1;
	}

	button[type='submit']:not([disabled]):hover:before {
		width: 200%;
		height: 200%;
		opacity: 1;
		animation: line-out 0.8s ease-out forwards;
	}

	@keyframes line-out {
		0% {
			transform: translate(-50%, -50%) scale(0);
		}
		100% {
			transform: translate(-50%, -50%) scale(1);
			opacity: 0;
		}
	}
</style>
