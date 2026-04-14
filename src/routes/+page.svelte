<script lang="ts">
	let mobileMenuOpen = $state(false);
	let donationAmount = $state('');
	let selectedPreset = $state<number | null>(null);
	let activeSection = $state('home');

	const presets = [10, 25, 50, 100];

	const stats = [
		{ label: 'Total Donations', value: '$2.4M' },
		{ label: 'ARA Distributed', value: '18.6M' },
		{ label: 'Campaigns', value: '340+' },
		{ label: 'Contributors', value: '52K+' }
	];

	const campaigns = [
		{
			id: 1,
			title: 'Clean Water for Villages',
			description: 'Provide safe drinking water to 50 rural villages across Southeast Asia.',
			raised: 14200,
			goal: 20000,
			reward: 500,
			category: 'Humanitarian',
			image: '💧'
		},
		{
			id: 2,
			title: 'Plant a Million Trees',
			description: 'Reforestation initiative to restore biodiversity and fight climate change.',
			raised: 33800,
			goal: 50000,
			reward: 1000,
			category: 'Environment',
			image: '🌳'
		},
		{
			id: 3,
			title: 'Education for All',
			description: 'Build schools and fund scholarships for underprivileged children worldwide.',
			raised: 8900,
			goal: 15000,
			reward: 250,
			category: 'Education',
			image: '📚'
		}
	];

	const navLinks = [
		{ id: 'home', label: 'Home' },
		{ id: 'campaigns', label: 'Campaigns' },
		{ id: 'how-it-works', label: 'How It Works' },
		{ id: 'leaderboard', label: 'Leaderboard' },
		{ id: 'about', label: 'About' }
	];

	const leaderboard = [
		{ rank: 1, name: 'CryptoPhilanthropist', donated: '$12,400', ara: '620,000 ARA', badge: '🥇' },
		{ rank: 2, name: 'GreenEarth_DAO', donated: '$9,800', ara: '490,000 ARA', badge: '🥈' },
		{ rank: 3, name: 'HopeBuilder99', donated: '$7,200', ara: '360,000 ARA', badge: '🥉' },
		{ rank: 4, name: 'BlockchainHeart', donated: '$5,100', ara: '255,000 ARA', badge: '🏅' },
		{ rank: 5, name: 'AnonDonor_042', donated: '$3,900', ara: '195,000 ARA', badge: '🏅' }
	];

	function selectPreset(amount: number) {
		selectedPreset = amount;
		donationAmount = String(amount);
	}

	function scrollTo(id: string) {
		activeSection = id;
		mobileMenuOpen = false;
		document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' });
	}

	function progressPct(raised: number, goal: number) {
		return Math.min(Math.round((raised / goal) * 100), 100);
	}
</script>

<!-- Navbar -->
<header class="fixed top-0 z-50 w-full border-b border-white/10 bg-slate-900/95 backdrop-blur">
	<div class="mx-auto flex max-w-7xl items-center justify-between px-4 py-3">
		<!-- Logo -->
		<a href="/" class="flex items-center gap-2 text-xl font-bold text-white">
			<span class="rounded-lg bg-violet-600 px-2 py-0.5 text-white">ARA</span>
			<span class="text-slate-200">Donate</span>
		</a>

		<!-- Desktop Nav -->
		<nav class="hidden items-center gap-6 md:flex">
			{#each navLinks as link}
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
			<button class="rounded-lg border border-slate-700 px-4 py-2 text-sm text-slate-300 transition hover:border-violet-500 hover:text-white">
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
			<span class="block h-0.5 w-6 bg-white transition-all {mobileMenuOpen ? 'translate-y-2 rotate-45' : ''}"></span>
			<span class="block h-0.5 w-6 bg-white transition-all {mobileMenuOpen ? 'opacity-0' : ''}"></span>
			<span class="block h-0.5 w-6 bg-white transition-all {mobileMenuOpen ? '-translate-y-2 -rotate-45' : ''}"></span>
		</button>
	</div>

	<!-- Mobile Menu -->
	{#if mobileMenuOpen}
		<div class="border-t border-white/10 bg-slate-900 px-4 py-4 md:hidden">
			{#each navLinks as link}
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
<section id="home" class="flex min-h-screen flex-col items-center justify-center bg-gradient-to-br from-slate-950 via-slate-900 to-violet-950 px-4 pt-20 text-center">
	<div class="inline-flex items-center gap-2 rounded-full border border-violet-500/40 bg-violet-500/10 px-4 py-1.5 text-sm text-violet-300">
		<span class="h-2 w-2 animate-pulse rounded-full bg-violet-400"></span>
		Earn ARA tokens with every donation
	</div>
	<h1 class="mt-6 max-w-3xl text-5xl font-extrabold leading-tight text-white md:text-6xl">
		Donate for Good,<br />
		<span class="bg-gradient-to-r from-violet-400 to-fuchsia-400 bg-clip-text text-transparent">
			Earn ARA Rewards
		</span>
	</h1>
	<p class="mt-5 max-w-xl text-lg text-slate-400">
		Every donation you make earns you ARA tokens — a crypto reward that grows with your generosity.
		Make an impact and get rewarded for it.
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
		{#each stats as stat}
			<div class="rounded-2xl border border-white/10 bg-white/5 p-5 backdrop-blur">
				<p class="text-2xl font-bold text-white">{stat.value}</p>
				<p class="mt-1 text-sm text-slate-400">{stat.label}</p>
			</div>
		{/each}
	</div>
</section>

<!-- Campaigns -->
<section id="campaigns" class="bg-slate-950 px-4 py-24">
	<div class="mx-auto max-w-7xl">
		<div class="mb-12 text-center">
			<h2 class="text-4xl font-bold text-white">Active Campaigns</h2>
			<p class="mt-3 text-slate-400">Pick a cause, donate, and earn ARA tokens instantly.</p>
		</div>

		<div class="grid gap-8 md:grid-cols-3">
			{#each campaigns as campaign}
				<div class="flex flex-col rounded-2xl border border-white/10 bg-slate-900 overflow-hidden transition hover:border-violet-500/50">
					<!-- Card Header -->
					<div class="flex items-center justify-between bg-slate-800 px-5 py-4">
						<span class="rounded-full bg-violet-500/20 px-3 py-0.5 text-xs font-medium text-violet-300">
							{campaign.category}
						</span>
						<span class="text-3xl">{campaign.image}</span>
					</div>

					<div class="flex flex-1 flex-col p-5">
						<h3 class="text-lg font-bold text-white">{campaign.title}</h3>
						<p class="mt-2 flex-1 text-sm text-slate-400">{campaign.description}</p>

						<!-- Progress -->
						<div class="mt-5">
							<div class="mb-1.5 flex justify-between text-xs text-slate-400">
								<span>${campaign.raised.toLocaleString()} raised</span>
								<span>{progressPct(campaign.raised, campaign.goal)}%</span>
							</div>
							<div class="h-2 w-full overflow-hidden rounded-full bg-slate-700">
								<div
									class="h-full rounded-full bg-gradient-to-r from-violet-500 to-fuchsia-500 transition-all"
									style="width: {progressPct(campaign.raised, campaign.goal)}%"
								></div>
							</div>
							<p class="mt-1 text-xs text-slate-500">Goal: ${campaign.goal.toLocaleString()}</p>
						</div>

						<!-- Reward badge -->
						<div class="mt-4 flex items-center gap-2 rounded-xl bg-violet-500/10 px-3 py-2">
							<span class="text-violet-400">✦</span>
							<span class="text-sm text-violet-300">Earn up to <strong>{campaign.reward.toLocaleString()} ARA</strong></span>
						</div>

						<button class="mt-4 w-full rounded-xl bg-violet-600 py-2.5 text-sm font-semibold text-white transition hover:bg-violet-500">
							Donate to This Campaign
						</button>
					</div>
				</div>
			{/each}
		</div>
	</div>
</section>

<!-- Quick Donate Widget -->
<section class="bg-gradient-to-r from-violet-900/40 to-fuchsia-900/40 px-4 py-16">
	<div class="mx-auto max-w-lg text-center">
		<h2 class="text-3xl font-bold text-white">Quick Donate</h2>
		<p class="mt-2 text-slate-400">Choose an amount and earn ARA tokens immediately.</p>

		<!-- Preset amounts -->
		<div class="mt-6 grid grid-cols-4 gap-3">
			{#each presets as preset}
				<button
					onclick={() => selectPreset(preset)}
					class="rounded-xl border py-3 text-sm font-semibold transition {selectedPreset === preset
						? 'border-violet-500 bg-violet-600 text-white'
						: 'border-slate-700 bg-slate-800 text-slate-300 hover:border-violet-500'}"
				>
					${preset}
				</button>
			{/each}
		</div>

		<!-- Custom amount -->
		<div class="mt-3 flex items-center gap-2">
			<span class="text-slate-400">$</span>
			<input
				type="number"
				bind:value={donationAmount}
				oninput={() => (selectedPreset = null)}
				placeholder="Custom amount"
				class="flex-1 rounded-xl border border-slate-700 bg-slate-800 px-4 py-3 text-white placeholder-slate-500 focus:border-violet-500 focus:outline-none"
			/>
		</div>

		{#if donationAmount}
			<div class="mt-4 rounded-xl bg-violet-500/10 p-3 text-sm text-violet-300">
				You'll earn approximately <strong>{Math.round(Number(donationAmount) * 50).toLocaleString()} ARA</strong> for this donation.
			</div>
		{/if}

		<button class="mt-4 w-full rounded-xl bg-violet-600 py-3.5 font-semibold text-white shadow-lg shadow-violet-900/50 transition hover:bg-violet-500 disabled:opacity-50" disabled={!donationAmount}>
			Donate & Earn ARA
		</button>
	</div>
</section>

<!-- How It Works -->
<section id="how-it-works" class="bg-slate-950 px-4 py-24">
	<div class="mx-auto max-w-5xl">
		<div class="mb-12 text-center">
			<h2 class="text-4xl font-bold text-white">How It Works</h2>
			<p class="mt-3 text-slate-400">Three simple steps to donate and start earning ARA tokens.</p>
		</div>

		<div class="grid gap-8 md:grid-cols-3">
			{#each [
				{ step: '01', icon: '🔗', title: 'Connect Your Wallet', desc: 'Link your crypto wallet (MetaMask, WalletConnect, etc.) to get started and receive ARA tokens.' },
				{ step: '02', icon: '💸', title: 'Choose & Donate', desc: 'Pick any active campaign, select your donation amount, and submit your contribution.' },
				{ step: '03', icon: '🪙', title: 'Receive ARA Tokens', desc: 'ARA tokens are automatically sent to your wallet. The more you donate, the more you earn.' }
			] as item}
				<div class="relative rounded-2xl border border-white/10 bg-slate-900 p-7">
					<span class="absolute right-5 top-5 text-5xl font-black text-slate-800">{item.step}</span>
					<div class="mb-4 text-4xl">{item.icon}</div>
					<h3 class="text-lg font-bold text-white">{item.title}</h3>
					<p class="mt-2 text-sm leading-relaxed text-slate-400">{item.desc}</p>
				</div>
			{/each}
		</div>

		<!-- ARA Token Info -->
		<div class="mt-12 rounded-2xl border border-violet-500/30 bg-violet-500/5 p-8">
			<div class="flex flex-col items-center gap-6 md:flex-row">
				<div class="flex h-20 w-20 shrink-0 items-center justify-center rounded-full bg-violet-600 text-3xl font-black text-white shadow-lg shadow-violet-900">
					ARA
				</div>
				<div>
					<h3 class="text-xl font-bold text-white">About the ARA Token</h3>
					<p class="mt-2 text-slate-400">
						ARA is a utility token issued on every verified donation. It can be used for governance voting on future campaigns,
						staked for additional rewards, or traded on supported DEXes. Every $1 donated earns you <strong class="text-violet-300">50 ARA</strong>.
					</p>
				</div>
			</div>
		</div>
	</div>
</section>

<!-- Leaderboard -->
<section id="leaderboard" class="bg-slate-900 px-4 py-24">
	<div class="mx-auto max-w-3xl">
		<div class="mb-10 text-center">
			<h2 class="text-4xl font-bold text-white">Top Donors</h2>
			<p class="mt-3 text-slate-400">Our most generous contributors and their ARA earnings.</p>
		</div>

		<div class="overflow-hidden rounded-2xl border border-white/10">
			{#each leaderboard as entry}
				<div class="flex items-center gap-4 border-b border-white/5 bg-slate-800/50 px-6 py-4 last:border-0 transition hover:bg-slate-800">
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
			Want to see your name here? <button onclick={() => scrollTo('campaigns')} class="text-violet-400 hover:underline">Start donating today.</button>
		</p>
	</div>
</section>

<!-- About -->
<section id="about" class="bg-slate-950 px-4 py-24">
	<div class="mx-auto max-w-3xl text-center">
		<h2 class="text-4xl font-bold text-white">About ARA Donate</h2>
		<p class="mt-5 text-lg leading-relaxed text-slate-400">
			ARA Donate is a decentralized donation platform that bridges generosity with blockchain-powered incentives.
			We believe that doing good should be recognized — and rewarded. By earning ARA tokens, donors build a stake
			in the ecosystem they're helping to create.
		</p>
		<p class="mt-4 text-slate-500">
			All donations are verified on-chain. Campaign funds are held in smart contracts and released to organizations
			upon milestone confirmation. Transparent. Trustless. Impactful.
		</p>
		<div class="mt-10 flex flex-wrap justify-center gap-4">
			<button class="rounded-xl bg-violet-600 px-6 py-3 font-semibold text-white transition hover:bg-violet-500">
				Read Whitepaper
			</button>
			<button class="rounded-xl border border-slate-700 px-6 py-3 font-semibold text-slate-300 transition hover:border-violet-500 hover:text-white">
				Join Community
			</button>
		</div>
	</div>
</section>

<!-- Footer -->
<footer class="border-t border-white/10 bg-slate-950 px-4 py-10 text-center">
	<div class="mx-auto max-w-7xl">
		<div class="flex flex-col items-center gap-4 md:flex-row md:justify-between">
			<div class="flex items-center gap-2 text-lg font-bold text-white">
				<span class="rounded-lg bg-violet-600 px-2 py-0.5 text-sm text-white">ARA</span>
				<span>Donate</span>
			</div>
			<nav class="flex flex-wrap justify-center gap-5 text-sm text-slate-500">
				{#each navLinks as link}
					<button onclick={() => scrollTo(link.id)} class="hover:text-slate-300">{link.label}</button>
				{/each}
			</nav>
			<p class="text-sm text-slate-600">© 2026 ARA Donate. All rights reserved.</p>
		</div>
	</div>
</footer>
