<script lang="ts">
	interface Props {
		name: string;
		role: string;
		avatar?: string;
		bio?: string;
		stats?: {
			followers: number;
			following: number;
			posts: number;
		};
	}
	let {
		name,
		role,
		avatar = '👤',
		bio,
		stats = { followers: 0, following: 0, posts: 0 }
	}: Props = $props();

	let isFollowing = $state(false);

	function toggleFollow() {
		isFollowing = !isFollowing;
	}
</script>

<div
	class="max-w-sm rounded-2xl border-2 border-gray-700 bg-gray-800 p-8 text-center transition-all duration-300 hover:-translate-y-1
  hover:border-blue-400 hover:shadow-2xl
  "
>
	<div class="mb-4 text-7xl">{avatar}</div>
	<h2 class="mb-2 text-2xl font-extrabold text-white">{name}</h2>
	<p class="m-0 mb-4 text-base font-semibold text-blue-400">{role}</p>
	{#if bio}
		<p class="mb-6 text-sm leading-relaxed text-gray-400">{bio}</p>
	{/if}
	<div class="mb-6 flex justify-around border-t border-b border-gray-700 py-5">
		<div class="flex flex-col gap-1">
			<span class="text-2xl font-extrabold text-white">{stats.followers}</span>
			<span class="text-xs font-semibold text-gray-500 uppercase">Followers</span>
		</div>
		<div class="flex flex-col gap-1">
			<span class="text-2xl font-extrabold text-white">{stats.posts}</span>
			<span class="text-xs font-semibold text-gray-500 uppercase">Posts</span>
		</div>
	</div>

	<button
		class="transitio-all w-full cursor-pointer rounded-lg border-none px-6 py-3.5 text-base font-bold duration-200"
		class:bg-blue-300={!isFollowing}
		class:text-black={!isFollowing}
		class:hover:bg-blue-300={!isFollowing}
		class:bg-green-400={isFollowing}
		class:hover:bg-green-300={isFollowing}
		class:hover:scale-[1.02]={true}
		onclick={toggleFollow}
	>
		{isFollowing ? '✓ Following' : '+ Follow'}
	</button>
</div>
