<script lang="ts">
	import type { Pathname } from '$app/types';
	import { resolve } from '$app/paths';
	import { page } from '$app/state';
	import { locales, localizeHref } from '$lib/paraglide/runtime.js';
	import './layout.css';

	const { children } = $props();
</script>

<div class="relative min-h-screen bg-slate-950">
	<div class="pointer-events-none fixed inset-0 overflow-hidden" aria-hidden="true">
		<div
			class="absolute -top-40 left-1/2 h-[600px] w-[600px] -translate-x-1/2 rounded-full bg-violet-600/10 blur-3xl"
		></div>
		<div
			class="absolute bottom-0 left-0 h-[400px] w-[400px] -translate-x-1/3 translate-y-1/3 rounded-full bg-fuchsia-600/8 blur-3xl"
		></div>
		<div
			class="absolute top-1/2 right-0 h-[300px] w-[300px] translate-x-1/3 -translate-y-1/2 rounded-full bg-violet-500/8 blur-3xl"
		></div>
		<div
			class="absolute inset-0 opacity-[0.03]"
			style="background-image: linear-gradient(rgba(139,92,246,0.5) 1px, transparent 1px), linear-gradient(90deg, rgba(139,92,246,0.5) 1px, transparent 1px); background-size: 64px 64px;"
		></div>
	</div>
	<div class="relative z-10">
		{@render children()}
	</div>
</div>

<div style="display:none">
	{#each locales as locale (locale)}
		<a href={resolve(localizeHref(page.url.pathname, { locale }) as Pathname)}>{locale}</a>
	{/each}
</div>
