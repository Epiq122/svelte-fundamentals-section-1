<script lang="ts">
	import ProfileCard from '$lib/components/ProfileCard.svelte';
</script>

<div class="flex min-h-screen flex-col items-center justify-center bg-gray-900 p-10">
	<ProfileCard
		name="Rob Gleason"
		role="Web Developer"
		avatar="👱‍♂️"
		bio="I build things for cool people"
		stats={{ followers: 1234, following: 500, posts: 99 }}
	/>
	<div class="fixed bottom-10 text-center">
		<a
			href="/"
			class="inline-block rounded-lg border-2 border-gray-700 bg-gray-800 px-8 py-4 font-semibold text-white transition-all duration-300 hover:-translate-y-1 hover:border-blue-400"
		>
			← Back to All Sections
		</a>
	</div>
</div>
