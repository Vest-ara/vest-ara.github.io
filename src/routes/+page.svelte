<script lang="ts">
	import { resolve } from '$app/paths';
	import logo from '$lib/assets/favicon.svg';
	import coin from '$lib/assets/coin.svg';
	let mobileMenuOpen = $state(false);
	// let donationAmount = $state('');
	// let selectedPreset = $state<number | null>(null);
	let activeSection = $state('home');

	// const presets = [10, 25, 50, 100];

	const stats = [
		{ id: 1, label: 'Total Donations', value: '$2.4M' },
		{ id: 2, label: 'ARA Distributed', value: '18.6M' },
		{ id: 3, label: 'Campaigns', value: '340+' },
		{ id: 4, label: 'Contributors', value: '52K+' }
	];

	// const campaigns = [
	// 	{
	// 		id: 1,
	// 		title: 'Clean Water for Villages',
	// 		description: 'Provide safe drinking water to 50 rural villages across Southeast Asia.',
	// 		raised: 14200,
	// 		goal: 20000,
	// 		reward: 500,
	// 		category: 'Humanitarian',
	// 		image: '💧'
	// 	},
	// 	{
	// 		id: 2,
	// 		title: 'Plant a Million Trees',
	// 		description: 'Reforestation initiative to restore biodiversity and fight climate change.',
	// 		raised: 33800,
	// 		goal: 50000,
	// 		reward: 1000,
	// 		category: 'Environment',
	// 		image: '🌳'
	// 	},
	// 	{
	// 		id: 3,
	// 		title: 'Education for All',
	// 		description: 'Build schools and fund scholarships for underprivileged children worldwide.',
	// 		raised: 8900,
	// 		goal: 15000,
	// 		reward: 250,
	// 		category: 'Education',
	// 		image: '📚'
	// 	}
	// ];

	const steps = [
		{
			id: 1,
			step: '01',
			icon: '🔗',
			title: 'Donate',
			desc: 'Donate some goods and we will pick them up from your location. We will verify the donation and issue ARA tokens to your wallet.'
		},
		{
			id: 2,
			step: '02',
			icon: '💸',
			title: 'Earn Rewards',
			desc: 'Earn ARA tokens with every donation and watch your ARA rewards grow with your generosity.'
		},
		{
			id: 3,
			step: '03',
			icon: '💱',
			title: 'Trade on Chain',
			desc: 'We will open an auction every 6 months for your goods to trade for they who need them.'
		}
	];

	const navLinks = [
		{ id: 'home', label: 'Home' },
		{ id: 'how-it-works', label: 'How It Works' },
		{ id: 'leaderboard', label: 'Leaderboard' },
		{ id: 'about', label: 'About' }
	];

	const leaderboard = [
		{ rank: 1, name: 'Pengu', donated: '$12,400', ara: '620,000 ARA', badge: '🥇' },
		{ rank: 2, name: 'GreenEarth_DAO', donated: '$9,800', ara: '490,000 ARA', badge: '🥈' },
		{ rank: 3, name: 'HopeBuilder99', donated: '$7,200', ara: '360,000 ARA', badge: '🥉' },
		{ rank: 4, name: 'BlockchainHeart', donated: '$5,100', ara: '255,000 ARA', badge: '🏅' },
		{ rank: 5, name: 'AnonDonor_042', donated: '$3,900', ara: '195,000 ARA', badge: '🏅' }
	];

	// function selectPreset(amount: number) {
	// 	selectedPreset = amount;
	// 	donationAmount = String(amount);
	// }

	function scrollTo(id: string) {
		activeSection = id;
		mobileMenuOpen = false;
		document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' });
	}

	// function progressPct(raised: number, goal: number) {
	// 	return Math.min(Math.round((raised / goal) * 100), 100);
	// }
</script>

<!-- Navbar -->
<header class="fixed top-0 z-50 w-full backdrop-blur">
	<div class="mx-auto flex max-w-7xl items-center justify-between px-4 py-3">
		<a href={resolve('/')} class="flex items-center gap-1 text-lg font-bold text-white">
			<!-- <span class="rounded-lg bg-violet-600 px-2 py-0.5 text-white"></span> -->
			<img src="{logo}" alt="Vestara Logo" class="h-10" />
			<span class="text-slate-200">Vestara</span>
		</a>

		<!-- Desktop Nav -->
		<nav class="hidden items-center gap-6 md:flex">
			{#each navLinks as link (link.id)}
				<button
					onclick={() => scrollTo(link.id)}
					class="text-sm font-medium transition-colors {activeSection === link.id
						? 'text-violet-400'
						: 'text-slate-400 hover:text-white'}"
				>
					{link.label}
				</button>
			{/each}
		</nav>

		<!-- CTA -->
		<div class="hidden items-center gap-3 md:flex">
			<button
				class="rounded-lg border border-slate-700 px-4 py-2 text-sm text-slate-300 transition hover:border-violet-500 hover:text-white"
			>
				Connect Wallet
			</button>
			<button
				onclick={() => scrollTo('campaigns')}
				class="rounded-lg bg-violet-600 px-4 py-2 text-sm font-semibold text-white transition hover:bg-violet-500"
			>
				Donate Now
			</button>
		</div>

		<!-- Mobile Hamburger -->
		<button
			class="flex flex-col gap-1.5 p-1 md:hidden"
			onclick={() => (mobileMenuOpen = !mobileMenuOpen)}
			aria-label="Toggle menu"
		>
			<span
				class="block h-0.5 w-6 bg-white transition-all {mobileMenuOpen
					? 'translate-y-2 rotate-45'
					: ''}"
			></span>
			<span class="block h-0.5 w-6 bg-white transition-all {mobileMenuOpen ? 'opacity-0' : ''}"
			></span>
			<span
				class="block h-0.5 w-6 bg-white transition-all {mobileMenuOpen
					? '-translate-y-2 -rotate-45'
					: ''}"
			></span>
		</button>
	</div>

	<!-- Mobile Menu -->
	{#if mobileMenuOpen}
		<div class="border-t border-white/10 bg-slate-900 px-4 py-4 md:hidden">
			{#each navLinks as link (link.id)}
				<button
					onclick={() => scrollTo(link.id)}
					class="block w-full py-2 text-left text-sm font-medium {activeSection === link.id
						? 'text-violet-400'
						: 'text-slate-300 hover:text-white'}"
				>
					{link.label}
				</button>
			{/each}
			<div class="mt-4 flex flex-col gap-2">
				<button class="w-full rounded-lg border border-slate-700 py-2 text-sm text-slate-300">
					Connect Wallet
				</button>
				<button
					onclick={() => scrollTo('campaigns')}
					class="w-full rounded-lg bg-violet-600 py-2 text-sm font-semibold text-white"
				>
					Donate Now
				</button>
			</div>
		</div>
	{/if}
</header>

<!-- Hero -->
<section
	id="home"
	class="flex min-h-screen flex-col items-center justify-center px-5 pt-20 text-center"
>
	<div
		class="inline-flex items-center rounded-full border border-violet-500/40 bg-violet-500/10 px-3 py-1.5 text-sm text-violet-300"
	>
		<span class="h-2 w-2 animate-pulse rounded-full bg-violet-400"></span>
		The first blockchain platform for branded secondhand goods donation.
	</div>
	<h1 class="mt-6 max-w-3xl text-5xl leading-tight font-extrabold text-white md:text-6xl">
		Donate for Good,<br />
		<span class="bg-gradient-to-r from-violet-400 to-fuchsia-400 bg-clip-text text-transparent">
			Earn ARA Rewards
		</span>
	</h1>
	<p class="mt-5 max-w-xl text-lg text-slate-400">
		Vestara is a Web3 platform built on Polygon where donors
		contribute branded secondhand goods and receive ARA
		(ARA) — a utility token — in return. Those tokens can then
		be used to claim other items in the Vestara marketplace.
	</p>
	<div class="mt-8 flex flex-wrap justify-center gap-4">
		<button
			onclick={() => scrollTo('campaigns')}
			class="rounded-xl bg-violet-600 px-7 py-3 font-semibold text-white shadow-lg shadow-violet-900/50 transition hover:bg-violet-500"
		>
			Start Donating
		</button>
		<button
			onclick={() => scrollTo('how-it-works')}
			class="rounded-xl border border-slate-700 px-7 py-3 font-semibold text-slate-300 transition hover:border-violet-500 hover:text-white"
		>
			Learn How It Works
		</button>
	</div>

	<!-- Stats -->
	<div class="mt-20 grid w-full max-w-4xl grid-cols-2 gap-4 md:grid-cols-4">
		{#each stats as stat (stat.id)}
			<div class="rounded-2xl border border-white/10 bg-white/5 p-5 backdrop-blur">
				<p class="text-2xl font-bold text-white">{stat.value}</p>
				<p class="mt-1 text-sm text-slate-400">{stat.label}</p>
			</div>
		{/each}
	</div>
</section>

<!-- How It Works -->
<section id="how-it-works" class="px-4 py-24">
	<div class="mx-auto max-w-5xl">
		<div class="mb-12 text-center">
			<h2 class="text-4xl font-bold text-white">How It Works</h2>
			<p class="mt-3 text-slate-400">Three simple steps to donate, earn and trade ARA tokens.</p>
		</div>

		<div class="grid gap-8 md:grid-cols-3">
			{#each steps as step (step.id)}
				<div class="relative rounded-2xl backdrop-blur p-5">
					<span class="absolute top-5 right-5 text-5xl font-black text-slate-800">{step.step}</span>
					<div class="mb-4 text-4xl">{step.icon}</div>
					<h3 class="text-lg font-bold text-white">{step.title}</h3>
					<p class="mt-2 text-sm leading-relaxed text-slate-400">{step.desc}</p>
				</div>
			{/each}
		</div>

		<!-- ARA Token Info -->
		<div class="mt-12 rounded-2xl border border-violet-500/30 bg-violet-500/5 p-8">
			<div class="flex flex-col items-center gap-6 md:flex-row">
				<div
					class="flex h-50 w-50 shrink-0 items-center justify-center rounded-full font-black text-white shadow-lg shadow-violet-900"
				>
					<img src="{coin}" alt="ARA Token Logo" />
				</div>
				<div>
					<h3 class="text-xl font-bold text-white">About the ARA Token</h3>
					<p class="mt-2 text-slate-400">
						ARA is a utility token issued on every verified donation. It can be used for governance
						voting on future campaigns, staked for additional rewards, or traded on supported DEXes.
						Every $1 donated earns you <strong class="text-violet-300">50 ARA</strong>.
					</p>
				</div>
			</div>
		</div>
	</div>
</section>

<!-- Leaderboard -->
<section id="leaderboard" class="px-4 py-24">
	<div class="mx-auto max-w-3xl">
		<div class="mb-10 text-center">
			<h2 class="text-4xl font-bold text-white">Top Donors</h2>
			<p class="mt-3 text-slate-400">Our most generous contributors and their ARA earnings.</p>
		</div>

		<div class="overflow-hidden rounded-2xl border border-white/10">
			{#each leaderboard as entry (entry.rank)}
				<div
					class="flex items-center gap-4 border-b border-white/5 bg-slate-800/50 px-6 py-4 transition last:border-0 hover:bg-slate-800"
				>
					<span class="w-8 text-xl">{entry.badge}</span>
					<div class="flex-1">
						<p class="font-semibold text-white">{entry.name}</p>
						<p class="text-xs text-slate-500">{entry.donated} donated</p>
					</div>
					<div class="text-right">
						<p class="text-sm font-bold text-violet-400">{entry.ara}</p>
					</div>
				</div>
			{/each}
		</div>

		<p class="mt-6 text-center text-sm text-slate-500">
			Want to see your name here? <button
				onclick={() => scrollTo('campaigns')}
				class="text-violet-400 hover:underline">Start donating today.</button
			>
		</p>
	</div>
</section>

<!-- About -->
<section id="about" class="px-4 py-24">
	<div class="mx-auto max-w-3xl text-center">
		<h2 class="text-4xl font-bold text-white">About VestAra</h2>
		<p class="mt-5 text-lg leading-relaxed text-slate-400">
			VestAra is a decentralized donation platform that bridges generosity with blockchain-powered
			incentives. We believe that doing good should be recognized — and rewarded. By earning ARA
			tokens, donors build a stake in the ecosystem they're helping to create.
		</p>
		<p class="mt-4 text-slate-500">
			All donations are verified on-chain. Campaign funds are held in smart contracts and released
			to organizations upon milestone confirmation. Transparent. Trustless. Impactful.
		</p>
		<div class="mt-10 flex flex-wrap justify-center gap-4">
			<button
				class="rounded-xl bg-violet-600 px-6 py-3 font-semibold text-white transition hover:bg-violet-500"
			>
				Read Whitepaper
			</button>
			<button
				class="rounded-xl border border-slate-700 px-6 py-3 font-semibold text-slate-300 transition hover:border-violet-500 hover:text-white"
			>
				Join Community
			</button>
		</div>
	</div>
</section>

<!-- Footer -->
<footer class=" px-4 py-10 text-center">
	<div class="mx-auto max-w-7xl">
		<div class="flex flex-col items-center gap-4 md:flex-row md:justify-between">
			<div class="flex items-center gap-1 text-lg font-bold text-white">
				<img src="{logo}" alt="Ara Logo" class="h-20" />
			</div>
			<nav class="flex flex-wrap justify-center gap-5 text-sm text-slate-500">
				{#each navLinks as link (link.id)}
					<button onclick={() => scrollTo(link.id)} class="hover:text-slate-300"
						>{link.label}</button
					>
				{/each}
			</nav>
			<p class="text-sm text-slate-600">© 2026 VestAra. All rights reserved.</p>
		</div>
	</div>
</footer>
