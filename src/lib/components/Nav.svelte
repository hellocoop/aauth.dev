<script>
	let mobileOpen = $state(false);

	function closeMobile() {
		mobileOpen = false;
	}

	const links = [
		{ href: '#why-aauth', label: 'Why AAuth' },
		{ href: '#how-it-works', label: 'How It Works' },
		{ href: '#get-started', label: 'Explore AAuth' },
		{ href: '#community', label: 'Community' }
	];
</script>

<nav
	class="fixed top-0 left-0 right-0 z-[100] flex items-center justify-between gap-4 px-5 md:px-8 py-3.5 border-b border-[var(--color-border)] bg-[var(--color-bg)]/90 backdrop-blur-md"
	aria-label="Main"
>
	<a
		href="/"
		class="font-display text-xl md:text-2xl font-bold tracking-tight text-[var(--color-accent)] no-underline shrink-0"
		onclick={closeMobile}
	>
		AAuth
	</a>

	<div class="hidden md:flex items-center gap-7 lg:gap-8">
		{#each links as link}
			<a
				href={link.href}
				class="text-sm font-medium text-[var(--color-text-muted)] hover:text-[var(--color-text)] transition-colors no-underline"
			>
				{link.label}
			</a>
		{/each}
	</div>

	<div class="flex md:hidden items-center gap-1">
		<button
			type="button"
			onclick={() => (mobileOpen = !mobileOpen)}
			class="flex h-10 w-10 items-center justify-center rounded-lg text-[var(--color-text-muted)] hover:bg-[var(--color-bg-card)] hover:text-[var(--color-text)] transition-colors"
			aria-expanded={mobileOpen}
			aria-controls="mobile-nav-menu"
			aria-label={mobileOpen ? 'Close menu' : 'Open menu'}
		>
			{#if mobileOpen}
				<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
				</svg>
			{:else}
				<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
				</svg>
			{/if}
		</button>
	</div>
</nav>

{#if mobileOpen}
	<button
		type="button"
		class="fixed inset-0 z-[90] bg-black/40 md:hidden cursor-default border-0 p-0"
		onclick={closeMobile}
		aria-label="Close menu"
	></button>
	<div
		id="mobile-nav-menu"
		class="fixed top-[57px] left-0 right-0 z-[95] md:hidden border-b border-[var(--color-border)] bg-[var(--color-bg)] shadow-lg"
	>
		<div class="flex flex-col px-5 py-3 gap-1">
			{#each links as link}
				<a
					href={link.href}
					class="py-2.5 text-base font-medium text-[var(--color-text)] hover:text-[var(--color-accent)] no-underline"
					onclick={closeMobile}
				>
					{link.label}
				</a>
			{/each}
		</div>
	</div>
{/if}
