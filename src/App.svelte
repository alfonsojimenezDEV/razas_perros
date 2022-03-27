<script>
	let raza = "Whippet";

	async function getDogs() {
		const url = 'https://dog.ceo/api/breed/' + `${raza.toLowerCase()}/images/random/1`;
		const res = await fetch(url);
		if (!res || res.status===404) return [];
		const json = await res.json();
		return json.message;
	}

	let dogsPromise = getDogs();

</script>

<main>
	<h1>App Razas de perros!</h1>
	<label for="">Raza
		<input type="text" bind:value={raza}>
	</label>
	<button on:click={() => dogsPromise = getDogs()}>Obtener imágenes</button>

	{#await dogsPromise}
		<!-- dogsPromise is pending -->
		<div>
			Esperando la carga de perros...
		</div>
	{:then imagesUrls}
		<!-- dogsPromise was fulfilled -->
		{#each imagesUrls as url}
			 <!-- content here -->
			 <div><img src="{url}" alt="dog"></div>
		{:else}
			 <!-- empty list -->
			 <div>Not found</div>
		{/each}
	{:catch error}
		<!-- dogsPromise was rejected -->
		<div>Error: {error.message}</div>
	{/await}

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>