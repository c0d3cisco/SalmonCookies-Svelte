<script>
	class CitySales {
		constructor(cityName, minCustomer, maxCustomer, avgCookieSaleHour) {
			this.cityName = cityName;
			this.minCustomer = minCustomer;
			this.maxCustomer = maxCustomer;
			this.avgCookieSaleHour = avgCookieSaleHour;
			this.hourlySaleList = [];
			this.hourlyEmployee = [];
			this.totalSales = 0;
		}

		generateHourlySales() {
			for (let i = 0; i < hours.length; i++) {
				let customers = Math.floor(Math.random() * (this.maxCustomer - this.minCustomer + 1)) + this.minCustomer;
				let hourlySale = Math.ceil(customers * this.avgCookieSaleHour);
				this.hourlySaleList.push(hourlySale);
			}
		}

		calculateTotalSales() {
			this.totalSales = this.hourlySaleList.reduce((total, sale) => total + sale, 0);
		}
	}

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
	export let cityObjArrInput;
	
	$: cityObjArrInput = cityObjArrInput.map((city) => {
		city = new CitySales(city.cityName, city.minCustomer, city.maxCustomer, city.avgCookieSaleHour)
		city.generateHourlySales();
		city.calculateTotalSales();
		return city;
	})

	let hourTotals = [];
  $: {
    hourTotals = [];
    for (let i = 0; i < hours.length; i++) {
      let total = 0;
      cityObjArrInput.forEach((city) => {
        total += city.hourlySaleList[i];
      });
      hourTotals.push(total);
    }
  }

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
		{#each cityObjArrInput as { cityName, hourlySaleList, totalSales }}
			<tbody>
				<tr>
					<td>{cityName}</td>
					{#each hourlySaleList as sale}
						<td>{sale}</td>
					{/each}
					<td>{totalSales}</td>
				</tr>
			</tbody>
		{/each}
		<tfoot>
      <tr>
        <td>Total</td>
        {#each hourTotals as total}
          <td>{total}</td>
        {/each}
        <td>{cityObjArrInput.reduce((total, city) => total + city.totalSales, 0)}</td>
      </tr>
    </tfoot>
	</table>
</main>

<style>
  /* Your CSS code goes here */
  table {
    width: 100%;
    border-collapse: collapse;
    border: 1px solid #ddd;
    margin-bottom: 20px;
    font-family: "Arial", sans-serif;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    border-radius: 6px;
  }

  th,
  td {
    padding: 12px 0;
    border-bottom: 1px solid #ddd;
    text-align: center;
    font-size: 16px; /* Adjust the font size as needed */
		border-right: 1px solid #dddddd8e;
  }

  th {
    background-color: #f2f2f2;
  }

  tfoot td {
    background-color: #f2f2f2;
    font-weight: bold;
    border-top: 2px solid #ddd;
  }

  tbody tr:hover {
    background: linear-gradient(to bottom, #f9f9f9, #f2f2f2);
  }

  td:hover {
    background-color: #f9f9f9;
  }

  tbody tr:nth-child(even) {
    background-color: #f2f2f2;
  }

  thead th {
    border-radius: 4px;
  }

</style>
