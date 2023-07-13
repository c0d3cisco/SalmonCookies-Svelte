<script>
	import Table from './table.svelte';

	let array = [
		{ cityName: 'FirstEntered', minCustomer: 23, maxCustomer: 65, avgCookieSaleHour: 6.3 },
		{ cityName: 'SecondEntered', minCustomer: 3, maxCustomer: 24, avgCookieSaleHour: 1.2 },
		{ cityName: 'SecondEntered', minCustomer: 3, maxCustomer: 24, avgCookieSaleHour: 1.2 },
		{ cityName: 'SecondEntered', minCustomer: 3, maxCustomer: 24, avgCookieSaleHour: 1.2 },
		{ cityName: 'SecondEntered', minCustomer: 3, maxCustomer: 24, avgCookieSaleHour: 1.2 },
	]

	let questions = [
		{
			id: 1,
			text: `Where did you go to school?`
		},
		{
			id: 2,
			text: `What is your mother's name?`
		},
		{
			id: 3,
			text: `What is another personal fact that an attacker could easily find with Google?`
		}
	];

	let selected;

	let answer = '';

	function handleSubmit() {
		alert(
			`answered question ${selected.id} (${selected.text}) with "${answer}"`
		);
	}

</script>

<!-- <main> include this later -->

	<!-- <Form /> Future Form -->
	<form on:submit|preventDefault={handleSubmit}>
		<select
			bind:value={selected}
			on:change={() => (answer = '')}
		>
			{#each questions as question}
				<option value={question}>
					{question.text}
				</option>
			{/each}
		</select>
	
		<input bind:value={answer} />
	
		<button disabled={!answer} type="submit">
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
