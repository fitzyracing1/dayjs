# patch-cloud build of dayjs@1.11.23

This tarball was built by patch-cloud from [fitzyracing1/dayjs@patch-cloud](https://github.com/fitzyracing1/dayjs/tree/patch-cloud),
a fork of [iamkun/dayjs](https://github.com/iamkun/dayjs). It is **not** an official upstream release.
Package name and version are unchanged from upstream (`dayjs@1.11.23`, base `tag v1.11.23`).

## Fix

timezone plugin: .tz() reuses one cached Intl.DateTimeFormat per zone instead of calling Date#toLocaleString (which builds a new formatter every call). About 5-6x faster per .tz() call with identical results.

- Upstream PR: https://github.com/iamkun/dayjs/pull/2019
- Credit: @logeyG (PR #2019 author; Co-authored-by in the fork commit)

## Exactly what changed vs upstream

- src/plugin/timezone/index.js -> plugin/timezone.js and esm/plugin/timezone/index.js: cached formatter (getLocaleStringifier from PR #2019) with exactly toLocaleString()'s default options, so the formatted string is identical. The PR's unrelated options refactor was not applied (it no longer applied cleanly).
- Every other built file is byte-identical to the published dayjs@1.11.23 tarball (locale.json differs only in entry order).
- Added this `PATCH-CLOUD.md` and a `_patchCloud` field in `package.json` (metadata only).

## Verification

- Formatter output equals Date#toLocaleString for 37,200 timestamps x 8 zones.
- Benchmark: 20,000 x dayjs(t).tz(zone): about 300 ms vs about 1,700 ms for upstream 1.11.23 (x5.8), identical formatted results.
- Upstream timezone tests pass in 4 TZs (54/54 each); full jest suite 792/794 with the 2 failures (localizedFormat, dayOfYear) identical on unmodified 1.11.23 when TZ=America/New_York and passing with TZ=UTC.

## Opting out

Remove the patch-cloud `registry=` override and reinstall from registry.npmjs.org to get the upstream tarball.
