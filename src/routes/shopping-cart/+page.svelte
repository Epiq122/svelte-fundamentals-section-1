<script lang="ts">
	interface CartItem {
		id: number;
		name: string;
		price: number;
		quantity: number;
		image: string;
	}

	let cart = $state<CartItem[]>([
		{ id: 1, name: 'Laptop', price: 999, quantity: 1, image: '💻' },
		{ id: 2, name: 'Mouse', price: 49, quantity: 2, image: '🖱️' },
		{ id: 3, name: 'Keyboard', price: 129, quantity: 1, image: '⌨️' }
	]);

	// derived values automatically recalculate
	const subtotal = $derived(cart.reduce((sum, item) => sum + item.price * item.quantity, 0));
	const tax = $derived(subtotal * 0.08);
	const total = $derived(subtotal + tax);
	const itemCount = $derived(cart.reduce((sum, item) => sum + item.quantity, 0));

	function updateQuantity(id: number, delta: number) {
		const item = cart.find((i) => i.id === id);
		if (item) {
			item.quantity = Math.max(0, item.quantity + delta);
			// remove if quantity is 0
			if (item.quantity === 0) {
				cart = cart.filter((i) => i.id !== id);
			}
		}
	}
	function removeItem(id: number) {
		cart = cart.filter((i) => i.id !== id);
	}
</script>

<div class="min-h-screen bg-gray-900 p-10 text-gray-200">
	<h1 class="m-0 mb-8 text-center text-4xl font-bold text-blue-400">🛒 Shopping Cart</h1>
	<div class="mx-auto mb-6 max-w-4xl text-right">
		<span class="rounded-full bg-gray-800 px-4 py-2 font-semibold text-blue-400"
			>{itemCount} {itemCount === 1 ? 'item' : 'items'}</span
		>
	</div>

	{#if cart.length === 0}
		<div class="px-5 py-20 text-center text-gray-500">
			<p>Your cart is empty</p>
			<p class="mt-5 text-6xl">🛍️</p>
		</div>
	{:else}
		<div class="mx-auto mb-8 max-w-4xl">
			{#each cart as item (item.id)}
				<div
					class="relative mb-4 flex items-center gap-5 rounded-xl border-2 border-gray-700 bg-gray-800 p-5"
				>
					<div class="text-5xl">{item.image}</div>
					<div class="flex-1">
						<h3 class="mb-1 text-lg">{item.name}</h3>
						<p class="text-sm text-gray-500">${item.price.toFixed(2)} each</p>
					</div>
					<div class="flex items-center gap-3">
						<button
							class="h-8 w-8 cursor-pointer rounded-md border-none bg-gray-700 text-lg transition-all duration-200 hover:bg-blue-400 hover:text-black"
							onclick={() => updateQuantity(item.id, -1)}>-</button
						>
						<span class="min-w-8 text-center text-lg font-bold">{item.quantity}</span>
						<button
							class="h-8 w-8 cursor-pointer rounded-md border-none bg-gray-700 text-lg transition-all duration-200 hover:bg-blue-400 hover:text-black"
							onclick={() => updateQuantity(item.id, 1)}>+</button
						>
					</div>
					<div class="min-w-[100px] text-right text-xl font-extrabold text-green-400">
						{(item.price * item.quantity).toFixed(2)}
					</div>
					<button
						class="absolute top-2.5 right-2.5 cursor-pointer border-none bg-transparent p-1 px-2 text-xl text-gray-600 hover:text-red-400"
						onclick={() => removeItem(item.id)}>x</button
					>
				</div>
			{/each}
		</div>
		<div class="mx-auto max-w-4xl rounded-xl border-2 border-gray-700 bg-gray-800 p-6">
			<div class="flex justify-between py-3 text-base">
				<span>Subtotal: </span>
				<span>${subtotal.toFixed(2)}</span>
			</div>
			<div class="flex justify-between py-3 text-base">
				<span>Tax (8%):</span>
				<span>${tax.toFixed(2)}</span>
			</div>
			<div
				class="mt-3 flex justify-between border-t-2 border-gray-700 pt-5 text-2xl font-extrabold text-blue-400"
			>
				<span>Total:</span>
				<span>${total.toFixed(2)}</span>
			</div>
			<button
				class="ronuded-lg mt-5 w-full cursor-pointer border-none bg-green-400 p-4 text-lg font-bold text-black transition-all duration-200 hover:-translate-y-0.5 hover:bg-green-300"
				>Proceed to Checkout</button
			>
		</div>
	{/if}

	<div class="mt-12 text-center">
		<a
			href="/"
			class="inline-block rounded-lg border-2 border-gray-700 bg-gray-800 px-8 py-4 font-semibold text-white transition-all duration-300 hover:-translate-y-1 hover:border-blue-400"
		>
			← Back to All Sections
		</a>
	</div>
</div>
