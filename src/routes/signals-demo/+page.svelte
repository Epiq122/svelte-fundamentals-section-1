<script lang="ts">
	// $state creates a signal -
	let count = $state(0);
	let name = $state('');
	let enabled = $state(true);

	// can be objects
	let user = $state({
		firstName: 'Rob',
		lastName: 'Gleason',
		age: 1
	});

	// arrays are reactive to
	let todos = $state<string[]>(['Learn Svelte', 'Build an app']);

	// derived signals automatically track dependencies
	const fullName = $derived(`${user.firstName} ${user.lastName}`);
	const isAdult = $derived(user.age >= 18);
	const todoCount = $derived(todos.length);

	function incrementAge() {
		user.age++;
	}

	function addTodo() {
		if (name.trim()) {
			todos.push(name);
			name = '';
		}
	}

	function removeTodo(index: number) {
		todos.splice(index, 1);
	}
</script>

<div class="min-h-screen bg-gray-900 p-10 text-gray-200">
	<h1 class="mb-10 text-center text-4xl font-bold text-blue-400">📡 Signal-Based Reactivity</h1>
	<div class="mx-auto grid max-w-4xl gap-6">
		<!-- Primitive Signals -->
		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="mb-5 text-2xl font-bold">Primitive Signals</h2>
			<div class="mb-4 flex items-center gap-4">
				<button
					class="cursor-pointer rounded-lg border-none bg-blue-400 px-6 py-3 font-bold text-black transition-all duration-200 hover:bg-blue-300"
					onclick={() => count++}>Increment: {count}</button
				>
				<label class="flex items-center gap-2">
					<input type="checkbox" bind:checked={enabled} class="h-5 w-5" />
					<span>Enabled: {enabled}</span>
				</label>
			</div>
		</div>
		<!-- Object Signals -->
		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="mb-5 text-2xl font-bold">Object Signals</h2>
			<div class="mb-4 flex gap-4">
				<input
					type="text"
					bind:value={user.firstName}
					placeholder="First Name"
					class="flex-1 rounded-lg border-2 border-gray-700 bg-gray-900 px-4 py-3"
				/>
				<input
					type="text"
					bind:value={user.lastName}
					placeholder="Last Name"
					class="flex-1 rounded-lg border-2 border-gray-700 bg-gray-900 px-4 py-3"
				/>
			</div>
			<div class="flex items-center gap-4">
				<button
					class="cursor-pointer rounded-lg border-none bg-green-400 px-6 py-3 font-bold text-black transition-all duration-200 hover:bg-green-300"
					onclick={incrementAge}>Age: {user.age}</button
				>
				<div>Full Name: <strong class="text-blue-400">{fullName}</strong></div>
				<div>{isAdult ? '✓ Adult' : '✗ Minor'}</div>
			</div>
		</div>

		<!-- Array Signals -->
		<div class="rouned-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="mb-5 text-2xl font-bold">Array Signals ({todoCount} items)</h2>
			<div class="mb-4 flex gap-3">
				<input
					type="text"
					bind:value={name}
					placeholder="Add a todo..."
					class="flex-1 rounded-lg border-2 border-gray-700 bg-gray-900 px-4 py-3"
					onkeydown={(e) => e.key === 'Enter' && addTodo()}
				/>
				<button
					onclick={addTodo}
					class="cursor-pointer rounded-lg border-none bg-blue-400 px-6 py-3 font-bold text-black transition-all duration-200 hover:bg-blue-300"
					>Add</button
				>
			</div>
			<ul class="list-none">
				{#each todos as todo, i}
					<li
						class="mb-2 flex items-center justify-between rounded-lg border-2 border-gray-700 bg-gray-900 p-4"
					>
						<span>{todo}</span>
						<button
							onclick={() => removeTodo(i)}
							class="cursor-pointer rounded border-none bg-red-400 px-4 py-2 text-black hover:bg-red-300"
							>Remove</button
						>
					</li>
				{/each}
			</ul>
		</div>
	</div>
	<div class="mt-12 text-center">
		<a
			href="/"
			class="inline-block rounded-lg border-2 border-gray-700 bg-gray-800 px-8 py-4 font-semibold text-white transition-all duration-300 hover:-translate-y-1 hover:border-blue-400"
		>
			← Back to All Sections
		</a>
	</div>
</div>
