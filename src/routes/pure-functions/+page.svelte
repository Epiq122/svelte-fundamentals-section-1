<script lang="ts">
	//  ✅ pure function
	function calculateTotal(price: number, taxRate: number): number {
		return price + price * taxRate;
	}
	// Demo State
	let price = $state(100);
	let taxRate = $state(0.08);
	const total = $derived(calculateTotal(price, taxRate));
</script>

<div class="min-h-screen bg-gray-900 p-10 text-gray-200">
	<h1 class="m-0 mb-10 text-center text-4xl text-blue-400">💰 Pure functions Demo</h1>
	<div class="mx-auto mb-6 max-w-2xl rounded-xl border-2 border-gray-700 bg-gray-800">
		<h2 class="m-2 text-2xl font-bold text-white">✅ Pure function example</h2>
		<div class="mb-4 flex flex-col gap-5">
			<label class="flex flex-col gap-2 font-semibold text-gray-400"
				>Price: ${price}
				<input type="range" class="w-full accent-blue-400" bind:value={price} min="0" max="200" />
			</label>
			<label class="flex flex-col gap-2 font-semibold text-gray-400">
				Tax Rate : {(taxRate * 100).toFixed(0)}%
				<input
					type="range"
					class="w-full accent-blue-400"
					bind:value={taxRate}
					min="0"
					max="0.2"
					step="0.01"
				/>
			</label>
		</div>
		<div class="mb-4 rounded-lg bg-blue-400 p-4 text-center text-2xl font-extrabold text-black">
			Total: ${total.toFixed(2)}
		</div>
		<p class="m-0 text-sm text-gray-500 italic">
			✅
			<code class="rounded bg-gray-900 px-1.5 py-0.5 font-mono text-blue-400">calculateTotal()</code
			>
			is pure: same inputs always product same output
		</p>
	</div>
	<div class="mx-auto mb-6 max-w-2xl rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
		<h2 class="m-0 mb-5 text-2xl font-bold text-white">Benefits of Pure Functions</h2>
		<ul class="m-0 pl-5">
			<li class="mb-3 leading-relaxed">🧪 <strong>Testable:</strong> Easy to unit test</li>
			<li class="mb-3 leading-relaxed">🔮 <strong>Predictable:</strong> No surprises</li>
			<li class="mb-3 leading-relaxed">🔄 <strong>Cacheable:</strong> Can memoize results</li>
			<li class="mb-3 leading-relaxed">🧩 <strong>Composable:</strong> Easy to combine</li>
			<li class="mb-3 leading-relaxed">🐛 <strong>Debuggable:</strong> No hidden dependencies</li>
		</ul>
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
