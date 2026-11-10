No account is needed.

- `getCve` returns one row per affected product, newest release first. A widely shipped Windows CVE
  can return dozens of rows; ask for fewer fields with `result <id> --fields product,severity,baseScore,kbArticles`.
- `severity` and `impact` are MSRC's labels (`Critical`, `Remote Code Execution`). `baseScore` and
  `temporalScore` are strings. `kbArticles` carries each KB's number, links and the fixed build.
- An id MSRC has not published returns `ok: true` with `data: []`.
- The request carries two random `request-id` headers that the page sends; they are generated per
  call and are not credentials.

## Maintainer notes

Learned 2026-11-10, logged out, from https://msrc.microsoft.com/update-guide/vulnerability/{cve}.
The holdout call and the no-cookie replay both answered over plain HTTP.

| op | args | returns | tier |
|---|---|---|---|
| `getCve` | `cve` (example `CVE-2023-23397`) | affected-product rows: `product`, `severity`, `impact`, `baseScore`, `vectorString`, `cweList`, `kbArticles` | 1 (GET to api.msrc.microsoft.com/sug/v2.0, under 1 s) |

```sh
api-anything call msrc getCve cve=CVE-2024-38063
```
