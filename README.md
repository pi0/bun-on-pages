# bun issue reproduction for cloudflare pages

## Issue

Cloudflare pages auto detects bun as runtime and package manager (which is amazing!)

Sadly detected version is old (1.0.1) and `packageManager` field in `package.json` (with `bun@1.0.25`) is not respected to use `1.0.25`.

1.0.25 has a fix for rollup v4 (https://github.com/oven-sh/bun/pull/7171) to be working and it breaks Nuxt, or any other project that uses rollup v4.
