<script lang="ts">
	import { untrack } from 'svelte';

	let count = $state(0);
	let doubled = $state(0);
	let tripled = $state(0);
	let renderCount = $state(0);

	// This runs AFTER the dom updates
	$effect(() => {
		// Track these values
		void count;
		void doubled;
		void tripled;

		// But don't track renderCount changes
		untrack(() => {
			renderCount++;
		});
	});
	function updateMultiple() {
		//  all three updates happen at once, in the microtask, DOM only updates once
		count++;
		doubled = count * 2;
		tripled = count * 3;
		console.log('updated queued, but dom not updated');
	}

	function updateWithDelay() {
		count++;

		// runs before next render
		queueMicrotask(() => {
			doubled = count * 2;
			console.log('Microtask executed');
		});

		//runs AFTER rnder
		setTimeout(() => {
			tripled = count * 3;
			console.log('Timeout executed (after render)');
		}, 0);
	}
</script>

<div class="min-h-screen bg-gray-900 p-10 text-gray-200">
	<h1 class="m-0 mb-10 text-center text-4xl font-bold text-blue-400">Microtask Queue Demo</h1>
	<div class="mx-auto mb-6 max-w-3xl rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
		<h2 class="m-0 mb-5 text-2xl font-bold text-white">State Values</h2>
		<div class="grid grid-cols-[repeat(auto-fit,minmax(150px,1fr))] gap-4">
			<div class="rounded-lg bg-gray-900 p-4 text-center">
				<span class="mb-2 block text-sm font-semibold">Count:</span>
				<span class="block text-3xl font-extrabold">{count}</span>
			</div>
			<div class="rounded-lg bg-gray-900 p-4 text-center">
				<span class="mb-2 block text-sm font-semibold">Doubled:</span>
				<span class="block text-3xl font-extrabold">{doubled}</span>
			</div>
			<div class="rounded-lg bg-gray-900 p-4 text-center">
				<span class="mb-2 block text-sm font-semibold">Tripled:</span>
				<span class="block text-3xl font-extrabold">{tripled}</span>
			</div>
			<div class="rounded-lg bg-gray-900 p-4 text-center">
				<span class="mb-2 block text-sm font-semibold">Render Count:</span>
				<span class="block text-3xl font-extrabold">{renderCount}</span>
			</div>
		</div>
	</div>

	<div class="mx-auto mb-6 max-w-3xl rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
		<h2 class="m-0 mb-5 text-2xl font-bold text-white">Try it out</h2>
		<div class="mb-4 flex gap-3">
			<!-- svelte-ignore a11y_consider_explicit_label -->
			<button
				onclick={updateMultiple}
				class="flex-1 cursor-pointer rounded-lg border-none bg-blue-400 px-6 py-3.5 text-base font-bold text-black transition-all duration-200 hover:-translate-y-0.5 hover:bg-blue-300"
				>Update All (Batched)</button
			>
			<button
				onclick={updateWithDelay}
				class="flex-1 cursor-pointer rounded-lg border-none bg-blue-400 px-6 py-3.5 text-base font-bold text-black transition-all duration-200 hover:-translate-y-0.5 hover:bg-blue-300"
				>Update with Microtask / Timeout</button
			>
		</div>
		<p class="m-0 rounded border-l-4 border-blue-400 bg-blue-400/10 px-4 py-3 text-sm">
			💡 Notice: Multiple state updates only cause ONE render! Svelte batches them automatically.
		</p>
	</div>
	<div class="mx-auto mb-6 max-w-3xl rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
		<h2 class="m-0 mb-5 text-2xl font-bold text-white">How It Works</h2>
		<ol class="m-0 pl-6">
			<li class="mb-3 leading-relaxed">
				<strong>Synchronous code runs:</strong> All state updates queued
			</li>
			<li class="mb-3 leading-relaxed">
				<strong>Microtasks run:</strong> queueMicrotask, Promises
			</li>
			<li class="mb-3 leading-relaxed"><strong>Render happens:</strong> DOM updates once</li>
			<li class="mb-3 leading-relaxed"><strong>Macrotasks run:</strong> setTimeout, setInterval</li>
		</ol>
	</div>
	<div class="fixed bottom-10 text-center">
		<a
			href="/"
			class="inline-block rounded-lg border-2 border-gray-700 bg-gray-800 px-8 py-4 font-semibold text-white transition-all duration-300 hover:-translate-y-1 hover:border-blue-400"
		>
			← Back to All Sections
		</a>
	</div>
</div>
