<script>
	let hours = [
		'6:00am',
		'7:00am',
		'8:00am',
		'9:00am',
		'10:00am',
		'11:00am',
		'12:00pm',
		'1:00pm',
		'2:00pm',
		'3:00pm',
		'4:00pm',
		'5:00pm',
		'6:00pm',
		'7:00pm'
	];
	let startCityObjArr = [
		{ cityName: 'Seattle', minCustomer: 23, maxCustomer: 65, avgCookieSaleHour: 6.3 },
		{ cityName: 'Tokyo', minCustomer: 3, maxCustomer: 24, avgCookieSaleHour: 1.2 },
		{ cityName: 'Dubai', minCustomer: 11, maxCustomer: 38, avgCookieSaleHour: 3.7 },
		{ cityName: 'Paris', minCustomer: 20, maxCustomer: 38, avgCookieSaleHour: 6.3 },
		{ cityName: 'Lima', minCustomer: 2, maxCustomer: 65, avgCookieSaleHour: 6.3 }
	];

	export let cityObjArr;

	$: cityObjArr = [
		...startCityObjArr,
		...cityObjArr,
	]

	let sum = Math.ceil(Math.random() * 100);
	let controlCurve = [0.5, 0.75, 1.0, 0.6, 0.8, 1.0, 0.7, 0.4, 0.6, 0.9, 0.68, 0.5, 0.3, 0.45];

	const returnTotal = () => {
		let total = 0;
		controlCurve.forEach((controlCurve) => {
			let randomNum = Math.floor(Math.random() * (sum - 1 + 1)) + 1;
			let hourlySale = Math.ceil(randomNum * sum * controlCurve);
			total += hourlySale;
		});
		return total;
	};
</script>

<main>
	<!-- Generate a table for each city in cityObjArr -->
	<h2>Table</h2>

	<table>
		<thead>
			<tr>
				<th>City</th>
				{#each hours as hour}
					<th>{hour}</th>
				{/each}
				<th id="thSalesTotal">Total</th>
			</tr>
		</thead>
		<!-- {#each cityObjArr as city, i(city.cityName)} -->
		{#each cityObjArr as { cityName: city }, i}
			<tbody>
				<tr>
					<td>{city}</td>
					{#each controlCurve as values}
						<td>{values}</td>
					{/each}
					<td>
						{returnTotal()}
					</td>
				</tr>
			</tbody>
		{/each}
	</table>
</main>

<style>
	/* Your CSS code goes here */
</style>
