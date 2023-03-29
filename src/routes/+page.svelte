<script>
	$: todos = [
		{
			id: 0,
			name: 'Todo 1',
			done: false
		},
		{
			id: 1,
			name: 'Todo 2',
			done: false
		},
		{
			id: 2,
			name: 'Todo 3',
			done: false
		},
		{
			id: 3,
			name: 'Todo 4',
			done: false
		}
	];
	$: selectedId = '';
	$: editMode = false;

	$: elems = {
		addInput: ''
	};

	const addToDo = () => {
		todos = [
			...todos,
			{
				id: todos.length,
				name: elems.addInput.value,
				done: false
			}
		];
		elems.addInput.value = '';
	};
	const swapUI = (id) => {
		elems.addInput.value = todos[id].name;
		editMode = true;
		selectedId = id;
	};
	const editToDo = () => {
		//map todos and replace with edited value
		todos = todos.map((todo, i) => {
			if (i == selectedId) {
				return {
					id: todo.id,
					name: elems.addInput.value,
					done: todo.done
				};
			} else return todo;
		});
		//swap UI back to normal
		elems.addInput.value = '';
		editMode = false;
	};
	const deleteToDo = (id) => {
		todos = todos.filter((todo) => todo.id !== id);
	};
	const markDone = (id) => {
		todos = todos.map((todo) => {
			if (todo.id == id) {
				return {
					id: todo.id,
					name: todo.name,
					done: !todo.done
				};
			} else return todo;
		});
		console.log(todos);
	};
	const clearDone = () => {};
</script>

<main>
	<div class="hero min-h-screen bg-base-300">
		<div class="hero-content card bg-base-200 shadow-xl">
			<div class="flex">
				<input
					bind:this={elems.addInput}
					type="text"
					placeholder="New todo..."
					class="input input-bordered input-primary w-full max-w-xs"
				/>
				{#if editMode}
					<button bind:this={elems.editBtn} on:click={editToDo} class="btn btn-primary ml-2"
						>Edit</button
					>
				{:else}
					<button bind:this={elems.addBtn} on:click={addToDo} class="btn btn-primary ml-2"
						>Add</button
					>
				{/if}
			</div>
			{#each todos as todo}
				{#if todo.done}
					<div class="card bg-green-500 p-4 w-full flex-row">
						<label class="label cursor-pointer mr-6">
							<input on:click={() => markDone(todo.id)} type="checkbox" class="checkbox" />
						</label>
						<p class="m-auto pr-10">{todo.name}</p>
						<button on:click={() => swapUI(todo.id)} class="btn btn-primary ml-6">Edit</button>
						<button on:click={() => deleteToDo(todo.id)} class="btn btn-primary ml-2">Delete</button
						>
					</div>
				{:else}
					<div class="card bg-base-100 p-4 w-full flex-row">
						<label class="label cursor-pointer mr-6">
							<input on:click={() => markDone(todo.id)} type="checkbox" class="checkbox" />
						</label>
						<p class="m-auto pr-10">{todo.name}</p>
						<button on:click={() => swapUI(todo.id)} class="btn btn-primary ml-6">Edit</button>
						<button on:click={() => deleteToDo(todo.id)} class="btn btn-primary ml-2">Delete</button
						>
					</div>
				{/if}
			{/each}
			<button class="btn btn-primary">Clear Done</button>
		</div>
	</div>
</main>
