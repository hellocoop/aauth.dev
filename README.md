# aauth.dev

Source for [www.aauth.dev](https://www.aauth.dev) — the public website for the AAuth protocol family.

AAuth is an authorization protocol for agent-to-resource authorization and identity claim retrieval. Every agent — any HTTP client — gets its own cryptographic identity. No pre-registration, no shared secrets, no dependency on a particular server.

## Architecture

- **Single-page site** built with [SvelteKit](https://svelte.dev) + [Tailwind CSS](https://tailwindcss.com), statically generated
- **Cloudflare Worker** for content negotiation — requests with `Accept: text/markdown` get markdown from R2, everything else gets the HTML site
- **Cloudflare R2** stores markdown versions of all content for AI agents and tools
- **`llms.txt`** at `/llms.txt` following the [llmstxt.org](https://llmstxt.org) spec

## Development

```bash
npm install
npm run dev
```

The dev server runs at `http://localhost:5173`.

## Build

```bash
npm run build
```

Static output goes to `build/`. Deploy to Cloudflare with:

```bash
npx wrangler deploy
```

## Contributing

We welcome contributions — new platform guides, corrections, suggestions, and improvements.

- **Open an issue** for bugs, questions, or feature requests: [github.com/hellocoop/aauth.dev/issues](https://github.com/hellocoop/aauth.dev/issues)
- **Submit a PR** for content or code changes
- **Add a platform guide** — if you've implemented AAuth in a language not listed on the site, we'd love a getting-started guide

### Content structure

| Path | What |
|------|------|
| `src/routes/+page.svelte` | The single-page homepage |
| `src/lib/components/` | Svelte components (Nav, InView, etc.) |
| `static/llms.txt` | Agent-facing content index |
| `worker/index.js` | Cloudflare Worker for content negotiation |

## Related repos

- **[dickhardt/AAuth](https://github.com/dickhardt/AAuth)** — Protocol specifications (IETF Internet-Drafts)
- **[hellocoop/AAuth](https://github.com/hellocoop/AAuth)** — TypeScript reference implementation
