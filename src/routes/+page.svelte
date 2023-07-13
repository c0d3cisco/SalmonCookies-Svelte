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
	<input bind:value={cityName} placeholder="City Name"/>
	<input type="number" min="0" step="0.1" bind:value={avgCookieSaleHour} on:wheel={handleScroll} placeholder="Average sales per hour"/>
	<input type="number" min="0" step="0.1" bind:value={maxCustomer} on:wheel={handleScroll} placeholder="Max customers per hour"/>
	<input type="number" min="0" step="0.1" bind:value={minCustomer} on:wheel={handleScroll} placeholder="Minimum customers per hour"/>

	<button disabled={!(minCustomer && maxCustomer && avgCookieSaleHour)} type="submit">
		Submit
	</button>
</form>

<div class="container">
	<Table cityObjArrInput={array} />
</div>

<style>
	/* Your CSS code goes here */
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

  input[type="number"] {
    width: 200px;
    padding: 10px;
    margin: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
		text-align: center;
  }

  button[type="submit"] {
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

  button[type="submit"]:disabled {
    background-color: #999;
    cursor: not-allowed;
  }

  button[type="submit"]:hover {
    background-color: #45a049;
  }

  .container {
    /* Add any additional styling for the table container if needed */
  }
</style>
