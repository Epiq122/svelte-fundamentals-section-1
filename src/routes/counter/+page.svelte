<script lang="ts">
	import { onMount } from 'svelte';

	// state

	let count = $state(0);
	let step = $state(1);
	let min = $state(-10);
	let max = $state(10);
	let history = $state<number[]>([0]);
	let autoIncrement = $state(false);

	// derived values
	const atMin = $derived(count <= min);
	const atMax = $derived(count >= max);
	const canUndo = $derived(history.length > 1);
	const average = $derived(
		history.length ? history.reduce((sum, val) => sum + val, 0) / history.length : 0
	);

	// status message with complex logic
	const statusMessage = $derived.by(() => {
		if (atMax) return '🔴 Maximum reached!';
		if (atMin) return '🔴 Minimum reached!';
		if (count > 0) return '🟢 Positive';
		if (count < 0) return '🔵 Negative';
		return '⚪️ Zero';
	});

	// Effects
	$effect(() => {
		document.title = `Counter: ${count}`;
	});

	$effect(() => {
		localStorage.setItem('counter-value', count.toString());
	});

	$effect(() => {
		if (!autoIncrement) return;

		const interval = setInterval(() => {
			if (count < max) {
				increment();
			} else {
				autoIncrement = false;
			}
		}, 100);
		return () => clearInterval(interval);
	});

	// functions
	function increment() {
		if (count + step <= max) {
			count += step;
			addToHistory();
		}
	}

	function decrement() {
		if (count - step >= min) {
			count -= step;
			addToHistory();
		}
	}

	function reset() {
		count = 0;
		history = [0];
	}
	function addToHistory() {
		history.push(count);
		if (history.length > 10) {
			history = history.slice(-10);
		}
	}

	function undo() {
		if (canUndo) {
			history.pop();
			count = history[history.length - 1];
		}
	}

	// load saved value
	onMount(() => {
		const saved = localStorage.getItem('counter-value');
		if (saved) {
			count = parseInt(saved, 10);
			history = [count];
		}
	});
</script>

<div class="min-h-screen bg-gray-900 p-10 text-gray-200">
	<h1 class="m-0 mb-10 text-center text-4xl font-bold text-blue-400">Complete Counter Component</h1>

	<div class="mx-auto max-w-4xl space-y-6">
		<!-- MaIn counter display -->
		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-8 text-center">
			<div class="mb-4 text-8xl font-bold">{count}</div>
			<div class="mb-6 text-xl text-gray-400">{statusMessage}</div>
			<div class="mb-4 flex justify-center gap-3">
				<button
					onclick={decrement}
					disabled={atMin}
					class="cursor-pointer rounded-lg border-none bg-red-400 px-8 py-4 text-xl font-bold text-black transition-all duration-200 hover:bg-red-300 disabled:cursor-not-allowed disabled:opacity-30 disabled:hover:bg-red-400"
					>- {step}</button
				>
				<button
					onclick={reset}
					class="cursor-pointer rounded-lg border-none bg-gray-700 px-8 py-4 text-xl font-bold text-white transition-all duration-200 hover:bg-gray-600"
					>Reset</button
				>
				<button
					onclick={increment}
					disabled={atMax}
					class="cursor-pointer rounded-lg border-none bg-green-400 px-8 py-4 text-xl font-bold text-black transition-all duration-200 hover:bg-green-300 disabled:cursor-not-allowed disabled:opacity-30 disabled:hover:bg-green-400"
					>+ {step}</button
				>
			</div>
			<div class="flex justify-center gap-3">
				<button
					onclick={undo}
					disabled={!canUndo}
					class="cursor-pointer rounded-lg border-none bg-blue-400 px-6 py-3 font-bold text-black transition-all duration-200 hover:bg-blue-300 disabled:cursor-not-allowed disabled:opacity-30"
					>Undo</button
				>
				<label class="flex cursor-pointer items-center gap-2 rounded-lg bg-gray-700 px-6 py-3">
					<input type="checkbox" bind:checked={autoIncrement} class="h-5 w-5" />
					<span class="font-bold">Auto Increment</span>
				</label>
			</div>
		</div>

		<!-- Controls -->
		<div class="grid grid-cols-3 gap-4">
			<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
				<label class="flex flex-col gap-2">
					<span class="font-semibold">Step: {step}</span>
					<input type="range" bind:value={step} min="1" max="5" class="w-full accent-blue-400" />
				</label>
			</div>
			<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
				<label class="flex flex-col gap-2">
					<span class="font-semibold">Min: {min}</span>
					<input type="range" bind:value={min} min="-20" max="0" class="w-full accent-red-400" />
				</label>
			</div>
			<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
				<label class="flex flex-col gap-2">
					<span class="font-semibold">Max: {max}</span>
					<input type="range" bind:value={max} min="0" max="20" class="w-full accent-green-400" />
				</label>
			</div>
		</div>
		<!-- stats -->
		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="m-0 mb-5 text-2xl font-bold text-white">Statistics (Derived)</h2>
			<div class="grid grid-cols-4 gap-4 text-center">
				<div class="rounded-lg bg-gray-900 p-4">
					<div class="text-3xl font-bold text-blue-400">{count}</div>
					<div class="text-sm text-gray-500">Current</div>
				</div>
				<div class="rounded-lg bg-gray-900 p-4">
					<div class="text-3xl font-bold text-blue-400">{history.length}</div>
					<div class="text-sm text-gray-500">History</div>
				</div>
				<div class="rounded-lg bg-gray-900 p-4">
					<div class="text-3xl font-bold text-blue-400">{max - min}</div>
					<div class="text-sm text-gray-500">Range</div>
				</div>
			</div>
		</div>
		<!-- history -->
		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="m-0 mb-5 text-2xl font-bold text-white">History (Last 10)</h2>
			<div class="flex flex-wrap gap-2">
				{#each history as value, i}
					<div
						class="rounded-lg px-4 py-2 font-bold"
						class:bg-blue-400={i === history.length - 1}
						class:text-black={i === history.length - 1}
						class:bg-gray-700={i !== history.length - 1}
						class:text-gray-400={i !== history.length - 1}
					>
						{value}
					</div>
				{/each}
			</div>
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
