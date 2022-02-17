<script>
	import Child from "./Child.svelte";

	export let name;

	console.log(name)

	const swapiBase = 'https://swapi.dev/api/'

	const getData = async () => {
		console.log('getting data')

		let response = await fetch(swapiBase + 'people');
		let data = await response.json();

		return data;
	}

	let promise = getData();
	

	let todos = [
		{
			id: 1,
			todo: 'tell Thor hi',
			done: false
		},
		{
			id: 2,
			todo: 'Eat',
			done: true
		},
		{
			id: 3,
			todo: 'Do homework',
			done: false
		},
	]

	let newTodo = ''

	const handleClick = (e) => {
		todos = [
			...todos,
			{
				id: todos.length,
				todo: newTodo,
				done: false
			}
		]
			//clear out the text box
			newTodo = ''
	}

	$: console.log(newTodo)
</script>

<main>
	<Child demoData='Lando' />

	
	<h1>Todo App</h1>
	<form>
		<input bind:value={newTodo} type="text">

		<button on:click|preventDefault={handleClick}>
			Add todo
		</button>
	</form>

	<ul>
		{#each todos as todo}
			<li><input bind:checked={todo.done} type="checkbox">{todo.todo}</li>
		{/each}
	</ul>

	<button on:click={() => { promise = getData()}}>get data</button>

	{#await promise}
	<!-- promise is pending -->
	<p>waiting for the promise to resolve...</p>
{:then value}
	<!-- promise was fulfilled -->
	<p>The value is {value}</p>

	{#each value.results as person}
		<p>{person.name}</p>
	{/each}

{:catch error}
	<!-- promise was rejected -->
	<p>Something went wrong: {error.message}</p>
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