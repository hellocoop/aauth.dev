<script>
	import '../app.css';
	let { children } = $props();
</script>

<svelte:head>
	<title>AAuth — Agent Identity, Resource Access, and User Delegation</title>
	<meta name="description" content="Every agent gets its own cryptographic identity. No pre-registration, no shared secrets, no dependency on a particular server." />
	<meta property="og:type" content="website" />
	<meta property="og:url" content="https://www.aauth.dev" />
	<meta property="og:title" content="AAuth — Agent Identity, Resource Access, and User Delegation" />
	<meta property="og:description" content="Every agent gets its own cryptographic identity. No pre-registration, no shared secrets, no dependency on a particular server." />
	<meta name="twitter:card" content="summary_large_image" />
</svelte:head>

{@render children()}
