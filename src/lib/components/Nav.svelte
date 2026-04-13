<script>
	import DecryptText from '$lib/components/DecryptText.svelte';

	let scrolled = $state(false);
	let menuOpen = $state(false);
	let logoTrigger = $state(0);

	$effect(() => {
		const onScroll = () => {
			scrolled = window.scrollY > 20;
		};
		window.addEventListener('scroll', onScroll, { passive: true });
		return () => window.removeEventListener('scroll', onScroll);
	});

	$effect(() => {
		document.body.style.overflow = menuOpen ? 'hidden' : '';
		return () => {
			document.body.style.overflow = '';
		};
	});

	const links = [
		{ href: '#compare', label: 'Why AAuth' },
		{ href: '#how-it-works', label: 'How It Works' },
		{ href: '#specs', label: 'Specs' },
		{ href: '#get-started', label: 'Get Started' }
	];
</script>

<nav
	class="fixed top-0 left-0 right-0 z-50 border-b border-transparent transition-[background-color,backdrop-filter,border-color] duration-300 {scrolled || menuOpen
		? 'bg-[var(--color-bg)]/90 backdrop-blur-md border-[var(--color-border)]'
		: ''}"
>
	<div class="max-w-6xl mx-auto px-6 h-16 flex items-center justify-between">
		<a
			href="/"
			onmouseenter={() => logoTrigger++}
			class="font-display font-bold text-xl tracking-tight text-white no-underline relative z-10"
		>
			<DecryptText text="AAuth" trigger={logoTrigger} />
		</a>

		<div class="hidden md:flex items-center gap-6 text-sm">
			{#each links as link}
				<a href={link.href} class="text-[var(--color-text-muted)] hover:text-white transition-colors no-underline">{link.label}</a>
			{/each}
			<a
				href="https://github.com/dickhardt/AAuth"
				target="_blank"
				rel="noopener"
				class="text-[var(--color-text-muted)] hover:text-white transition-colors no-underline"
			>
				GitHub ↗
			</a>
		</div>

		<button
			class="md:hidden relative z-10 p-2 -mr-2 text-[var(--color-text)] cursor-pointer"
			aria-label={menuOpen ? 'Close menu' : 'Open menu'}
			aria-expanded={menuOpen}
			onclick={() => (menuOpen = !menuOpen)}
		>
			<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
				{#if menuOpen}
					<line x1="6" y1="6" x2="18" y2="18" />
					<line x1="6" y1="18" x2="18" y2="6" />
				{:else}
					<line x1="4" y1="7" x2="20" y2="7" />
					<line x1="4" y1="12" x2="20" y2="12" />
					<line x1="4" y1="17" x2="20" y2="17" />
				{/if}
			</svg>
		</button>
	</div>

</nav>

{#if menuOpen}
	<div class="md:hidden" style="position: fixed; top: 4rem; left: 0; right: 0; bottom: 0; background: var(--color-bg); z-index: 40;">
		<div class="flex flex-col gap-1 px-6 py-8">
			{#each links as link}
				<a
					href={link.href}
					onclick={() => (menuOpen = false)}
					class="text-lg py-3 text-[var(--color-text)] no-underline border-b border-[var(--color-border)]"
				>
					{link.label}
				</a>
			{/each}
			<a
				href="https://github.com/dickhardt/AAuth"
				target="_blank"
				rel="noopener"
				onclick={() => (menuOpen = false)}
				class="text-lg py-3 text-[var(--color-text)] no-underline"
			>
				GitHub ↗
			</a>
		</div>
	</div>
{/if}
