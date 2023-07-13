<script>
	import Table from './table.svelte';

	let array = [
		{ cityName: 'Seattle', minCustomer: 23, maxCustomer: 65, avgCookieSaleHour: 6.3 },
		{ cityName: 'Tokyo', minCustomer: 3, maxCustomer: 24, avgCookieSaleHour: 1.2 },
		{ cityName: 'Dubai', minCustomer: 11, maxCustomer: 38, avgCookieSaleHour: 3.7 },
		{ cityName: 'Paris', minCustomer: 20, maxCustomer: 38, avgCookieSaleHour: 6.3 },
		{ cityName: 'Lima', minCustomer: 2, maxCustomer: 65, avgCookieSaleHour: 6.3 },
	];

	let cityName;
	let minCustomer;
	let maxCustomer;
	let avgCookieSaleHour;
	$: minCustomer = minCustomer > maxCustomer ? maxCustomer : minCustomer;

	function handleSubmit(e) {
		array = array.filter((city) => city.cityName === cityName).length
			? array.map((city) =>
					city.cityName === cityName
						? { cityName, minCustomer, maxCustomer, avgCookieSaleHour }
						: city
			  )
			: [...array, { cityName, minCustomer, maxCustomer, avgCookieSaleHour }];
	}

	function handleScroll(event) {
		event.target.value = parseFloat(event.target.value) + event.deltaY * 0.01;
	}
</script>

<!-- <main> include this later -->

<!-- <Form /> Future Form -->
<form on:submit|preventDefault={handleSubmit}>
	<input bind:value={cityName} />
	<input type="number" min="0" step="0.1" bind:value={avgCookieSaleHour} on:wheel={handleScroll} />
	<input type="number" min="0" step="0.1" bind:value={maxCustomer} on:wheel={handleScroll} />
	<input type="number" min="0" step="0.1" bind:value={minCustomer} on:wheel={handleScroll} />

	<button disabled={!(minCustomer && maxCustomer && avgCookieSaleHour)} type="submit">
		Submit
	</button>
</form>

<div class="container">
	<Table cityObjArrInput={array} />
</div>

<style>
	/* Your CSS code goes here */
</style>
