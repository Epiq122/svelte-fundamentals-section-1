<script lang="ts">
	import { onMount } from 'svelte';

	let count = $state(0);
	let name = $state('User');
	let theme = $state<'light' | 'dark'>('dark');
	let notifications = $state<string[]>([]);

	// effect runs when count changes
	$effect(() => {
		document.title = `Count:${count}`;
		console.log('Title updated: ', count);
	});

	// Effect with cleanup
	$effect(() => {
		const interval = setInterval(() => {
			console.log(`Current count: ${count}`);
		}, 5000);

		// cleanup function
		return () => {
			clearInterval(interval);
			console.log('interval cleared');
		};
	});

	// effect for localstorage sync
	$effect(() => {
		localStorage.setItem('user-name', name);
		console.log('Saved to localStorage', name);
	});

	// effect for theme
	$effect(() => {
		document.body.classList.toggle('light-theme', theme === 'light');
		console.log('Theme changed:', theme);
	});

	// load from localStorage on mount
	onMount(() => {
		const savedName = localStorage.getItem('user-name');
		if (savedName) {
			name = savedName;
		}
	});
	function addNotification() {
		const message = `Action at ${new Date().toLocaleDateString()}`;
		notifications.push(message);

		// auto-remove after 3 seconds
		setTimeout(() => {
			notifications = notifications.filter((n) => n !== message);
		}, 3000);
	}

	// track when notifications change

	$effect(() => {
		if (notifications.length > 0) {
			console.log('Notifications:', notifications.length);
		}
	});
</script>

<div class="min-h-screen bg-gray-900 p-10 text-gray-200">
	<h1 class="m-0 mb-10 text-center text-4xl font-bold text-blue-400">💫 Effects Demo</h1>
	<!-- DOcument title effect -->
	<div class="mx-auto max-w-4xl space-y-6">
		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="text-2xl font-bold">Document Title Effect</h2>
			<p class="mb-4 text-gray-400">Count changes update the browser tab title!</p>
			<div class="flex gap-3">
				<button
					onclick={() => count++}
					class="cursor-pointer rounded-lg border-none bg-blue-400 px-6 py-3 font-bold text-black transition-all duration-200 hover:bg-blue-300"
					>Increment {count}</button
				>
				<button
					onclick={() => (count = 0)}
					class="cursor-pointer rounded-lg border-none bg-gray-700 px-6 py-3 font-bold text-black transition-all duration-200 hover:bg-gray-600"
					>Reset</button
				>
			</div>
		</div>
		<!-- LocalStorage Effect -->
		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="mb-5 text-2xl font-bold">LocalStorage Sync</h2>
			<p class="mb-4 text-gray-400">Name is automatically saved to LocalStorage!</p>
			<input
				type="text"
				bind:value={name}
				placeholder="Enter your name"
				class="w-full rounded-lg border-2 border-gray-700 bg-gray-900 px-4 py-3"
			/>
			<p class="mt-2 text-sm text-gray-500">Synced to LocalStorage automatically</p>
		</div>

		<!-- Theme effect -->
		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="m-0 mb-5 text-2xl font-bold text-white">Theme Effect</h2>
			<p class="mb-4 text-gray-400">Effect updates document.body class!</p>
			<div class="flex gap-3">
				<button
					onclick={() => (theme = 'dark')}
					class="cursor-pointer rounded-lg border-none px-6 py-3 font-bold transition-all duration-200"
					class:bg-blue-400={theme === 'dark'}
					class:text-black={theme === 'dark'}
					class:bg-gray-700={theme !== 'dark'}
					class:text-white={theme !== 'dark'}>Dark</button
				>
				<button
					onclick={() => (theme = 'light')}
					class="cursor-pointer rounded-lg border-none px-6 py-3 font-bold transition-all duration-200"
					class:bg-blue-400={theme === 'light'}
					class:text-black={theme === 'light'}
					class:bg-gray-700={theme !== 'light'}
					class:text-white={theme !== 'light'}>Light</button
				>
			</div>
		</div>

		<!-- notifications with cleanup -->

		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="m-0 mb-5 text-2xl font-bold text-white">
				Notifications ({notifications.length})
			</h2>
			<button
				onclick={addNotification}
				class="mb-4 cursor-pointer rounded-lg border-none bg-green-400 px-6 py-3 font-bold text-black transition-all duration-200 hover:bg-green-300"
				>Add Notification (auto-removes in 3s)</button
			>
			<div class="soace-y-2">
				{#each notifications as notification}
					<div class="mt-2 rounded-lg border-l-4 border-green-400 bg-gray-900 p-4">
						{notification}
					</div>
				{/each}
			</div>
		</div>

		<!-- Console logging -->
		<div class="rounded-xl border-2 border-blue-400 bg-blue-400/10 p-6">
			<h3 class="m-0 mb-3 font-bold text-blue-400">💡 Open DevTools Console</h3>
			<p class="m-0 text-sm">Effects log messages when they run. Check the console to see them!</p>
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
