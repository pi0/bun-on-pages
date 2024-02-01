# bun issue reproduction for cloudflare pages

## Issue

Cloudflare pages auto detects bun as runtime and package manager (which is amazing!)

Sadly detected version is old (1.0.1) and `packageManager` field in `package.json` (with `bun@1.0.25`) is not respected to use `1.0.25`.

1.0.25 has a fix for rollup v4 (https://github.com/oven-sh/bun/pull/7171) to be working and it breaks Nuxt, or any other project that uses rollup v4.

## Deploy

- Create new pages project
- Framework: empty
- Build: `bun run build`
- Deploy dir: `dist`

## Current Workarounds

Use `bun upgrade && bun run build` as build command

## Some more context

when using `bun <command>` CLI, bun by default it uses Node.js when there is `#/bin/env node` shebang which is case for 99% of npm pacakge binaries. Essentially in most of cases bun is used only as package manager and Node.js as real runtime. `bun --bun` or `bun <script>` or `#/bin/env bun` are the ways to use actually use bun.

## Possible Fixes

Cloudflare pages builder could:

- Respect Bun version in `packageManager` field
- Install latest 1.x version of Bun
- At least upgrade to current 1.0.25 to resolve major issues with rollup v4 support

## Logs

Issue:

```
Detected the following tools from environment: bun@1.0.1, nodejs@18.17.1
```

Full:

```
2024-02-01T17:43:34.572503836Z	Cloning repository...
2024-02-01T17:43:35.334332451Z	From https://github.com/pi0/bun-on-pages
2024-02-01T17:43:35.334640246Z	 * branch            b347242113dd61e7ba219ca6754f9cbf93786302 -> FETCH_HEAD
2024-02-01T17:43:35.334845114Z
2024-02-01T17:43:35.367270886Z	HEAD is now at b347242 update lock
2024-02-01T17:43:35.367525983Z
2024-02-01T17:43:35.466038643Z
2024-02-01T17:43:35.46641083Z	Using v2 root directory strategy
2024-02-01T17:43:35.497197771Z	Success: Finished cloning repository files
2024-02-01T17:43:36.259661418Z	Detected the following tools from environment: bun@1.0.1, nodejs@18.17.1
2024-02-01T17:43:36.260417069Z	Installing project dependencies: bun install --frozen-lockfile
2024-02-01T17:43:36.535165538Z	bun install v1.0.1 (31aec4eb)
2024-02-01T17:43:36.708061354Z	 + rollup@4.9.6
2024-02-01T17:43:36.708268599Z
2024-02-01T17:43:36.708474308Z	 4 packages installed [179.00ms]
2024-02-01T17:43:36.712486667Z	Executing user command: bun run build
2024-02-01T17:43:37.01420895Z	$ bun ./build.mjs
2024-02-01T17:43:37.085411895Z	Bun version 1.0.1
2024-02-01T17:43:37.085450547Z	Importing rollup
2024-02-01T17:43:37.112536307Z	1 | const { existsSync } = require('node:fs');
2024-02-01T17:43:37.113607573Z	2 | const { join } = require('node:path');
2024-02-01T17:43:37.113636155Z	3 | const { platform, arch, report } = require('node:process');
2024-02-01T17:43:37.113641816Z	4 |
2024-02-01T17:43:37.113646072Z	5 | const isMusl = () => !report.getReport().header.glibcVersionRuntime;
2024-02-01T17:43:37.113657567Z	                                          ^
2024-02-01T17:43:37.113661515Z	TypeError: undefined is not an object (evaluating 'report.getReport')
2024-02-01T17:43:37.11366506Z	      at isMusl (/opt/buildhome/repo/node_modules/rollup/dist/native.js:5:39)
2024-02-01T17:43:37.114786515Z	      at getPackageBase (/opt/buildhome/repo/node_modules/rollup/dist/native.js:60:27)
2024-02-01T17:43:37.114808067Z	      at /opt/buildhome/repo/node_modules/rollup/dist/native.js:35:20
2024-02-01T17:43:37.114813715Z	      at globalThis (/opt/buildhome/repo/node_modules/rollup/dist/native.js:102:33)
2024-02-01T17:43:37.114817943Z	      at processTicksAndRejections (:1:2602)
2024-02-01T17:43:37.118430633Z	error: script "build" exited with code 1 (SIGHUP)
2024-02-01T17:43:37.118576697Z	Failed: Error while executing user command. Exited with error code: 1
2024-02-01T17:43:37.125288087Z	Failed: build command exited with code: 1
```
