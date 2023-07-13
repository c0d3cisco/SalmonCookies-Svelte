<script>
	import Table from './table.svelte';

	let array = [
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
	<h1>Table</h1>

	<Table cityObjArr={array} />
</div>

<style>
	/* Your CSS code goes here */
</style>
