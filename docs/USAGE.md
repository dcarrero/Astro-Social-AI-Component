# Usage & configuration

`astro-social-ai-component` is a single Astro component (`ShareBar.astro`) plus a data table
(`platforms.ts`). It renders a compact row of buttons that deep-link the current page to social
networks and to AI assistants (with a "summarize this article" prompt). Everything is configured
through props — you never edit the component.

## Install

Copy `src/ShareBar.astro` and `src/platforms.ts` into your project (e.g. `src/components/`), or add
the package as a local/git dependency and import:

```astro
import ShareBar from 'astro-social-ai-component/ShareBar.astro';
```

## Minimal example

```astro
---
import ShareBar from '../components/ShareBar.astro';
const title = entry.data.title;
const url = new URL(`/blog/${entry.data.slug}/`, Astro.site).href; // canonical, absolute
---
<ShareBar title={title} url={url} lang="en" handle="@you" />
```

Place it **above** (right below the title) and/or **below** the article body — just render it twice.

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `title` | `string` | — | Article title used in share text. **Required.** |
| `url` | `string` | — | Canonical **absolute** URL. **Required.** |
| `handle` | `string` | — | X/Twitter handle, e.g. `@you`. Renders as `title via @you`. |
| `via` | `boolean` | `true` | Append `via @handle` on X. Set `false` to just append the handle. |
| `platforms` | `string[]` | all | Which platforms and in what order (keys below). |
| `labels` | `boolean` | `true` | `false` = icon-only (most compact, ideal for ~2 lines). |
| `lang` | `string` | `'en'` | One of the 25 built-in languages (below). |
| `strings` | `Partial<Strings>` | — | Override individual strings (see Custom strings). |
| `heading` | `string \| null` | from `lang` | Override just the heading; `null`/`''` hides it. |
| `class` | `string` | — | Extra class on the `<section>`. |

## Platforms

Social: `twitter`, `linkedin`, `facebook`, `whatsapp`, `telegram`, `email`.
AI: `chatgpt`, `claude`, `gemini`, `perplexity`, `grok`, `deepseek`, `mistral`, `copilot`,
`google_ai`.

Default is **all** (interleaved). Pick a subset and order:

```astro
<ShareBar title={title} url={url}
  platforms={["twitter", "linkedin", "whatsapp", "chatgpt", "claude", "perplexity"]} />
```

## Internationalization

`lang` selects from 25 built-in languages — `en`, `es`, `fr`, `de`, `it`, `pt`, `nl`, `pl`, `ru`,
`uk`, `tr`, `ar`, `he`, `fa`, `hi`, `zh`, `ja`, `ko`, `id`, `ms`, `vi`, `th`, `sv`, `cs`, `el`.
Each defines the heading, the `Share on` / `Summarize in` button prefixes, the `via` word, and the
**AI prompt** sent to each assistant (which always cites the page URL as the source).

```astro
<ShareBar title={title} url={url} lang="ja" />
```

### Custom strings

Override any field (merges over the chosen `lang`):

```astro
<ShareBar title={title} url={url} lang="en"
  strings={{
    heading: 'Spread the word',
    aiPrompt: (u) => `Summarize this post in 5 bullet points and cite it. Source: ${u}`,
  }} />
```

Add a brand-new language by adding a key to `STRINGS` in `platforms.ts`.

## Theming

Styles are scoped and driven by CSS variables with fallbacks; the bar inherits a host theme that
defines `--ink`, `--muted`, `--accent`, `--border-strong`, `--surface`, `--accent-soft`,
`--font-mono`, otherwise it uses neutral defaults. Override per instance or globally:

```css
.share-bar { --sb-accent: #2563eb; --sb-border: #e5e7eb; --sb-bg: #fff; }
```

AI buttons carry `.share-bar__btn.is-ai` (dashed border by default) to distinguish them from social
ones. On phones (≤520px) labels hide automatically to keep the bar to ~2 lines.

## Add a platform

In `platforms.ts`, append an entry to `PLATFORMS`:

```ts
mychat: {
  key: 'mychat', label: 'MyChat', kind: 'ai',
  icon: 'M…',                                  // 24×24 path, fill=currentColor
  href: (c) => `https://mychat.example/?q=${encodeURIComponent(c.prompt)}`,
},
```

…then add its key to `SHARE_DEFAULT` (or pass it in `platforms`). AI entries receive `c.prompt`;
social entries use `c.title` / `c.url` (and `c.via` / `c.handle` for X).
