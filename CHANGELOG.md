# Changelog

All notable changes to **astro-social-ai-component**. Format based on
[Keep a Changelog](https://keepachangelog.com/); versioning [SemVer](https://semver.org/).

## [0.2.1] — 2026-06-28

### Docs
- `docs/preview.svg`: preview now shows **all 15 supported platforms** with the **English**
  heading ("Share or summarize with AI"), matching the package's default language. The heading is
  localized for all 25 languages and can be hidden with `heading={null}`.

## [0.2.0] — 2026-06-28

### Added
- **i18n with 25 built-in languages** (`lang` prop): en, es, fr, de, it, pt, nl, pl, ru, uk, tr,
  ar, he, fa, hi, zh, ja, ko, id, ms, vi, th, sv, cs, el. Each ships the heading, the
  `Share on`/`Summarize in` prefixes, the `via` word and a localized AI summary prompt.
- **`strings` prop** to override any text (merges over the chosen language), incl. a custom
  `aiPrompt(url)`.
- **`via` prop** (default true): X/Twitter text renders as `title via @handle` using the language's
  `via` word; set false to just append the handle.
- `docs/USAGE.md`: full English usage & configuration guide.

### Changed
- Default `lang` is `en`. The AI prompt and labels are no longer hardcoded — they come from
  `STRINGS[lang]`.

## [0.1.0] — 2026-06-28

### Added
- Initial extraction from the carrero.es theme.
- `ShareBar.astro`: compact share + “summarize with AI” bar. Props: `title`, `url`, `handle`,
  `platforms`, `labels`, `heading`, `class`. Self-contained scoped styles with `--sb-*` CSS
  variable fallbacks; icon-only on phones.
- `platforms.ts`: 15 platforms (6 social, 9 AI) with per-platform deep-link builders and a shared
  AI summary prompt that cites the source URL. `SHARE_DEFAULT` order; `PLATFORMS` table.
- README with usage, props, theming and customization.
