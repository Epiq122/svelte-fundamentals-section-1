<script lang="ts">
	interface Product {
		name: string;
		price: number;
		quantity: number;
	}

	let products = $state<Product[]>([
		{ name: 'Laptop', price: 999, quantity: 2 },
		{ name: 'Mouse', price: 29, quantity: 5 },
		{ name: 'Keyboard', price: 79, quantity: 3 }
	]);

	let discountPercent = $state(10);
	let taxRate = $state(8);

	// Simple derived value
	const subtotal = $derived(products.reduce((sum, p) => sum + p.price * p.quantity, 0));

	// Chained derived values
	const discount = $derived(subtotal * (discountPercent / 100));
	const afterDiscount = $derived(subtotal - discount);
	const tax = $derived(afterDiscount * (taxRate / 100));
	const total = $derived(afterDiscount + tax);

	// Derived with objects
	const stats = $derived({
		itemCount: products.reduce((sum, p) => sum + p.quantity, 0),
		productCount: products.length,
		averagePrice: products.length ? subtotal / products.reduce((sum, p) => sum + p.quantity, 0) : 0
	});

	// Derived with complex logic
	const savingsMessage = $derived.by(() => {
		if (discount > 100) return `🎉 you're saving over $100!`;
		if (discount < 50) return `💰 Great savings of $${discount.toFixed(2)}!`;
		if (discount > 0) return `✔️ saving $${discount.toFixed(2)}`;
		return `🎫 Add a discount code to save!`;
	});

	function updateQuantity(index: number, delta: number) {
		products[index].quantity = Math.max(0, products[index].quantity + delta);
	}
</script>

<div class="min-h-screen bg-gray-900 p-10 text-gray-200">
	<h1 class="m-0 mb-10 text-center text-4xl font-bold text-blue-400">Derived State Demo</h1>

	<div class="mx-auto max-w-4xl">
		<!-- products -->
		<div class="mb-6 rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="mb-5 text-2xl font-bold">Products</h2>
			{#each products as product, i}
				<div class="mt-4 flex items-center gap-4 rounded-lg bg-gray-900 p-4">
					<span class="flex-1 font-semibold">{product.name}</span>
					<span class="text-gray-400">${product.price}</span>
					<button
						class="cursoor-pointer h-8 w-8 rounded border-none bg-gray-700 hover:bg-blue-400"
						onclick={() => updateQuantity(i, -1)}
					>
						-
					</button>
					<span class="w-8 text-center font-bold">{product.quantity}</span>
					<button
						class="cursoor-pointer h-8 w-8 rounded border-none bg-gray-700 hover:bg-blue-400"
						onclick={() => updateQuantity(i, 1)}
					>
						+
					</button>
					<span class="ml-4 min-w-20 text-right font-bold text-green-400"
						>${(product.price * product.quantity).toFixed(2)}</span
					>
				</div>
			{/each}
		</div>
		<!-- controls -->
		<div class="mb-6 grid grid-cols-2 gap-4">
			<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
				<label class="flex flex-col gap-2">
					<span class="font-semibold">Discount: {discountPercent}%</span>
					<input
						class="w-full accent-blue-400"
						type="range"
						bind:value={discountPercent}
						min="0"
						max="50"
					/>
				</label>
			</div>
			<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
				<label class="flex-col gap-2">
					<span class="font-semibold">Tax Rate: {taxRate}%</span>
					<input
						class="w-full accent-blue-400"
						type="range"
						bind:value={taxRate}
						min="0"
						max="50"
					/>
				</label>
			</div>
		</div>

		<!-- Stats -->
		<div class="mb-6 rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<div class="grid grid-cols-3 gap-4 text-center">
				<div class="rounded-lg bg-gray-900 p-4">
					<div class="text-3xl font-bold text-blue-400">{stats.itemCount}</div>
					<div class="text-sm text-gray-500">Total Items</div>
				</div>
				<div class="rounded-lg bg-gray-900 p-4">
					<div class="text-3xl font-bold text-blue-400">{stats.productCount}</div>
					<div class="text-sm text-gray-500">Total Types</div>
				</div>
				<div class="rounded-lg bg-gray-900 p-4">
					<div class="text-3xl font-bold text-blue-400">{stats.averagePrice.toFixed(2)}</div>
					<div class="text-sm text-gray-500">Avg Price/Item</div>
				</div>
			</div>
		</div>
		<div class="rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<h2 class="mb-5 text-2xl font-bold">Price Breakdown</h2>
			<div class="space-y-3">
				<div class="flex justify-between text-lg">
					<span>Subtotal:</span>
					<span>${subtotal.toFixed(2)}</span>
				</div>
				<div class="flex justify-between text-lg">
					<span>Discount ({discountPercent}%):</span>
					<span>-${discount.toFixed(2)}</span>
				</div>
				<div class="flex justify-between text-lg">
					<span>Tax ({taxRate}%)</span>
					<span>${tax.toFixed(2)}</span>
				</div>
				<div
					class="flex justify-between border-t-2 border-gray-700 pt-3 text-2xl font-bold text-green-400"
				>
					<span>Total:</span>
					<span>${total.toFixed(2)}</span>
				</div>
			</div>
			<div class="mt-4 rounded border-l-4 border-blue-400 bg-blue-400/10 p-4">
				<p class="text-sm">{savingsMessage}</p>
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
