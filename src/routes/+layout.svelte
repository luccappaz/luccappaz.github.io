<script lang="ts">
	import { resolve, asset } from '$app/paths';
	import { browser } from '$app/environment';
	import './layout.css';
	let { children } = $props();
	const links = [
		{ href: '/#home', label: 'HOME' },
		{ href: '/#projects', label: 'PROJECTS' },
		{ href: '/#stack', label: 'STACK' },
		{ href: '/#certifications', label: 'CERTIFICATIONS' },
		{ href: '/#about', label: 'ABOUT' },
		{ href: '/#contact', label: 'CONTACT' }
	] as const;
	// tema: lê do localStorage no client, senão cai para 'dark'
	let theme = $state<'dark' | 'light'>(
		browser ? ((localStorage.getItem('theme') as 'dark' | 'light') ?? 'dark') : 'dark'
	);
	$effect(() => {
		document.documentElement.setAttribute('data-theme', theme);
		localStorage.setItem('theme', theme);
	});
	function toggleTheme() {
		theme = theme === 'dark' ? 'light' : 'dark';
	}

	let mobileMenuOpen = $state(false);
</script>
<svelte:head>
	<title>Lucca | Data Engineer</title>
	<meta
		name="description"
		content="Data Engineer • Machine Learning • Distributed Systems"
	/>

	<link rel="icon" type="image/svg+xml" href={asset('/favicon.svg')} />

	<meta property="og:title" content="Lucca | Data Engineer" />
	<meta property="og:description" content="Data Engineer • Machine Learning • Distributed Systems" />
	<meta property="og:image" content={asset('/og-image.png')} />
	<meta property="og:type" content="website" />

	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:title" content="Lucca | Data Engineer" />
	<meta name="twitter:description" content="Data Engineer • Machine Learning • Distributed Systems" />
	<meta name="twitter:image" content={asset('/og-image.png')} />
</svelte:head>
<div class="min-h-screen bg-bg-950 text-text-100 font-mono">
	<header
		class="sticky top-0 z-50 backdrop-blur-xl border-b border-primary-400/20 bg-bg-900/70"
	>
	<div class="max-w-7xl mx-auto grid grid-cols-[auto_1fr_auto] items-center px-4 md:px-8 py-5">

		<!-- Logo -->
		<a
			href={resolve('/')}
			data-text="LUCCA"
			class="glitch cyber-title text-2xl md:text-3xl text-primary-400 tracking-[0.3em]"
		>
			&gt;L<span class="animate-pulse">_</span>
		</a>

		<!-- Links -->
		<nav class="hidden md:flex justify-center gap-8">
			{#each links as link (link)}
				<a
					href={resolve(link.href)}
					class="uppercase tracking-[0.25em] text-xs text-text-300 hover:text-primary-400 transition"
				>
					&gt; {link.label}
				</a>
			{/each}
		</nav>

		<!-- Botões -->
		<div class="hidden md:flex items-center gap-3 justify-self-end">
			<button
				onclick={toggleTheme}
				aria-label="Toggle theme"
				class="cyber-button"
			>
				{theme === 'dark' ? '☀ LIGHT' : '🌙 DARK'}
			</button>

			<a
				href={asset('/lucca_cv.pdf')}
				download
				target="_blank"
				rel="noopener noreferrer"
				class="cyber-button"
			>
				DOWNLOAD CV
			</a>
		</div>

		<!-- Mobile -->
		<button
			class="md:hidden justify-self-end text-primary-400 text-2xl"
			aria-label="Menu"
			onclick={() => (mobileMenuOpen = !mobileMenuOpen)}
		>
			{mobileMenuOpen ? '✕' : '☰'}
		</button>

	</div>

		<!-- menu mobile -->
		{#if mobileMenuOpen}
			<nav class="md:hidden flex flex-col gap-4 px-4 pb-6 border-t border-primary-400/20">
				{#each links as link (link)}
					<a href={resolve(link.href)}
						onclick={() => (mobileMenuOpen = false)}
						class="uppercase tracking-[0.25em] text-xs text-text-300 hover:text-primary-400 transition pt-4"
					>
						&gt; {link.label}
					</a>
				{/each}
				<button
					onclick={toggleTheme}
					class="cyber-button mt-2"
				>
					{theme === 'dark' ? '☀ LIGHT' : '🌙 DARK'}
				</button>
				<a	href={asset('/lucca_cv.pdf')}
					download
					target="_blank"
					rel="noopener noreferrer"
					class="cyber-button text-center"
				>
					DOWNLOAD CV
				</a>
			</nav>
		{/if}
	</header>
	<main class="max-w-7xl mx-auto px-4 md:px-8 py-8 md:py-12">
		{@render children()}
	</main>
	<footer class="border-t border-primary-400/20 mt-20">
		<div
			class="max-w-7xl mx-auto px-4 md:px-8 py-8 flex flex-col md:flex-row gap-4 justify-between items-center text-xs text-text-500 text-center md:text-left"
		>
			<div>
				<div class="text-primary-400">DATA ENGINEER</div>
				<div>BUILDING DATA SYSTEMS</div>
			</div>
			<div class="text-right md:text-right">
				<div>POWERED BY SVELTEKIT</div>
				<div>© {new Date().getFullYear()} LUCCA DA PAZ</div>
			</div>
		</div>
	</footer>
</div>
