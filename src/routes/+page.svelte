<script lang="ts">
	interface Demo {
		title: string;
		description: string;
		path: string;
		emoji: string;
	}

	const demos: Demo[] = [
		{
			title: 'Pure Functions',
			description: 'Learn about pure functions and side effects',
			path: '/pure-functions',
			emoji: '🔬'
		},
		{
			title: 'Microtask Queue',
			description: 'Understanding JavaScript event loop and microtasks',
			path: '/microtask-queue',
			emoji: '⚡'
		},
		{
			title: 'Profile Card',
			description: 'Reactive state management with user profiles',
			path: '/profile-demo',
			emoji: '👤'
		},
		{
			title: 'Shopping Cart',
			description: 'Building a reactive shopping cart',
			path: '/shopping-cart',
			emoji: '🛒'
		},
		{
			title: 'Signal-Based Reactivity',
			description: 'Understanding $state and $derived',
			path: '/signals-demo',
			emoji: '📡'
		},
		{
			title: 'Derived State',
			description: 'Computed values with $derived',
			path: '/derived-demo',
			emoji: '🧮'
		},
		{
			title: 'Effects',
			description: 'Side effects with $effect',
			path: '/effects-demo',
			emoji: '💫'
		},
		{
			title: 'Counter Component',
			description: 'Complete reactive counter example',
			path: '/counter',
			emoji: '🔢'
		}
	];
</script>

<div class="min-h-screen bg-gray-900 p-8 text-gray-200">
	<div class="mx-auto max-w-7xl">
		<div class="mb-12 text-center">
			<h1 class="mb-4 text-5xl font-bold text-blue-400">Section 1: Fundamentals & Reactivity</h1>
			<p class="text-xl text-gray-400">Master the core concepts of Svelte 5 reactivity</p>
		</div>

		<div class="grid grid-cols-1 gap-6 md:grid-cols-2 lg:grid-cols-3">
			{#each demos as demo}
				<a
					href={demo.path}
					class="block rounded-xl border-2 border-gray-700 bg-gray-800 p-6 transition-all duration-300 hover:-translate-y-2 hover:border-blue-400 hover:shadow-[0_12px_40px_rgba(0,0,0,0.5)]"
				>
					<div class="mb-4 text-5xl">{demo.emoji}</div>
					<h2 class="mb-3 text-2xl font-bold text-white">{demo.title}</h2>
					<p class="leading-relaxed text-gray-400">{demo.description}</p>
				</a>
			{/each}
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
</div>
