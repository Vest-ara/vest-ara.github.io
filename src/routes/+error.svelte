<script lang="ts">
	import { page } from '$app/state';
	import { goto } from '$app/navigation';
	import { onMount, onDestroy } from 'svelte';
	import { resolve } from '$app/paths';

	const REDIRECT_SECONDS = 10;

	let countdown = $state(REDIRECT_SECONDS);
	let cancelled = $state(false);
	let redirecting = $state(false);
	let interval: ReturnType<typeof setInterval> | null = null;

	const progress = $derived(((REDIRECT_SECONDS - countdown) / REDIRECT_SECONDS) * 100);

	const is404 = $derived(page.status === 404);

	const errorConfig = $derived(
		is404
			? {
					code: '404',
					title: 'Page Not Found',
					description:
						"The page you're looking for doesn't exist or has been moved to another URL.",
					hint: 'Double-check the URL or head back to the homepage.'
				}
			: {
					code: String(page.status),
					title: 'Something Went Wrong',
					description:
						page.error?.message ??
						'An unexpected server error occurred. Our team has been notified.',
					hint: 'Please try again later or return to the homepage.'
				}
	);

	function cancel() {
		cancelled = true;
		if (interval) {
			clearInterval(interval);
			interval = null;
		}
	}

	function goHome() {
		redirecting = true;
		goto(resolve('/'));
	}

	onMount(() => {
		interval = setInterval(() => {
			countdown -= 1;
			if (countdown <= 0) {
				clearInterval(interval!);
				interval = null;
				redirecting = true;
				goto(resolve('/'));
			}
		}, 1000);
	});

	onDestroy(() => {
		if (interval) clearInterval(interval);
	});
</script>

<svelte:head>
	<title>{errorConfig.code} — {errorConfig.title}</title>
</svelte:head>

<div class="flex min-h-screen flex-col items-center justify-center px-4 py-16">
	<!-- Card -->
	<div
		class="w-full max-w-lg rounded-2xl border border-white/10 bg-slate-900/80 p-8 shadow-2xl backdrop-blur-sm sm:p-10"
	>
		<!-- Status badge -->
		<div class="mb-6 flex items-center gap-3">
			<span
				class="inline-flex items-center gap-1.5 rounded-full border px-3 py-1 text-xs font-medium {is404
					? 'border-violet-500/40 bg-violet-500/10 text-violet-300'
					: 'border-red-500/40 bg-red-500/10 text-red-300'}"
			>
				<span
					class="h-1.5 w-1.5 rounded-full {is404 ? 'bg-violet-400' : 'bg-red-400'} animate-pulse"
				></span>
				{is404 ? 'Not Found' : 'Server Error'}
			</span>
		</div>

		<!-- Error code -->
		<div class="mb-2 select-none">
			<span
				class="bg-gradient-to-br {is404
					? 'from-violet-300 via-violet-400 to-fuchsia-400'
					: 'from-red-300 via-rose-400 to-red-500'} bg-clip-text text-8xl leading-none font-black tracking-tight text-transparent sm:text-9xl"
			>
				{errorConfig.code}
			</span>
		</div>

		<!-- Title & description -->
		<h1 class="mb-2 text-2xl font-bold text-white sm:text-3xl">{errorConfig.title}</h1>
		<p class="mb-1 text-sm leading-relaxed text-slate-400">{errorConfig.description}</p>
		<p class="mb-8 text-xs text-slate-500">{errorConfig.hint}</p>

		<!-- Divider -->
		<div class="mb-6 h-px bg-white/5"></div>

		<!-- Auto-redirect section -->
		{#if !cancelled}
			<div class="mb-6">
				<div class="mb-3 flex items-center justify-between text-xs">
					<span class="text-slate-400">
						{#if redirecting}
							<span class="text-violet-400">Redirecting…</span>
						{:else}
							Redirecting to home in
							<span class="font-semibold text-white tabular-nums">{countdown}s</span>
						{/if}
					</span>
					<button
						onclick={cancel}
						class="rounded px-2 py-0.5 text-slate-500 transition-colors hover:text-slate-300 focus-visible:outline focus-visible:outline-2 focus-visible:outline-violet-500"
					>
						Cancel
					</button>
				</div>

				<!-- Progress bar -->
				<div class="h-1.5 w-full overflow-hidden rounded-full bg-slate-800">
					<div
						class="h-full rounded-full bg-gradient-to-r from-violet-500 to-fuchsia-500 transition-[width] duration-1000 ease-linear"
						style="width: {progress}%"
					></div>
				</div>
			</div>
		{:else}
			<div class="mb-6 flex items-center gap-2 text-xs text-slate-500">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 16 16"
					fill="currentColor"
					class="h-3.5 w-3.5"
				>
					<path d="M3.75 7.25a.75.75 0 0 0 0 1.5h8.5a.75.75 0 0 0 0-1.5h-8.5Z" />
				</svg>
				Auto-redirect cancelled
			</div>
		{/if}

		<!-- Action buttons -->
		<div class="flex flex-col gap-3 sm:flex-row">
			<button
				onclick={goHome}
				disabled={redirecting}
				class="flex flex-1 items-center justify-center gap-2 rounded-xl bg-violet-600 px-5 py-3 text-sm font-semibold text-white shadow-lg shadow-violet-500/20 transition-all hover:bg-violet-500 hover:shadow-violet-500/30 focus-visible:outline focus-visible:outline-2 focus-visible:outline-violet-400 active:scale-95 disabled:cursor-not-allowed disabled:opacity-60"
			>
				{#if redirecting}
					<svg
						class="h-4 w-4 animate-spin"
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
					>
						<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"
						></circle>
						<path
							class="opacity-75"
							fill="currentColor"
							d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"
						></path>
					</svg>
					Going home…
				{:else}
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 20 20"
						fill="currentColor"
						class="h-4 w-4"
					>
						<path
							d="M10.707 2.293a1 1 0 0 0-1.414 0l-7 7a1 1 0 0 0 1.414 1.414L4 10.414V17a1 1 0 0 0 1 1h2a1 1 0 0 0 1-1v-2a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1v2a1 1 0 0 0 1 1h2a1 1 0 0 0 1-1v-6.586l.293.293a1 1 0 0 0 1.414-1.414l-7-7Z"
						/>
					</svg>
					Go to Homepage
				{/if}
			</button>

			<button
				onclick={() => history.back()}
				class="flex flex-1 items-center justify-center gap-2 rounded-xl border border-white/10 bg-slate-800/60 px-5 py-3 text-sm font-medium text-slate-300 transition-all hover:border-white/20 hover:bg-slate-800 hover:text-white focus-visible:outline focus-visible:outline-2 focus-visible:outline-violet-400 active:scale-95"
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 20 20"
					fill="currentColor"
					class="h-4 w-4"
				>
					<path
						fill-rule="evenodd"
						d="M17 10a.75.75 0 0 1-.75.75H5.612l4.158 3.96a.75.75 0 1 1-1.04 1.08l-5.5-5.25a.75.75 0 0 1 0-1.08l5.5-5.25a.75.75 0 1 1 1.04 1.08L5.612 9.25H16.25A.75.75 0 0 1 17 10Z"
						clip-rule="evenodd"
					/>
				</svg>
				Go Back
			</button>
		</div>
	</div>

	<!-- Footer note -->
	<p class="mt-6 text-center text-xs text-slate-600">
		If this keeps happening, please
		<a
			href="mailto:support@dhyslexia@gmail.com"
			class="text-slate-500 underline-offset-2 transition-colors hover:text-slate-400 hover:underline"
		>
			contact support
		</a>
	</p>
</div>
