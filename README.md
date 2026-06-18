# evergreendesignbook.com

Landing page for *Evergreen Design: The Human Side of Design in the AI Age* by
Milovan Jovicic. Currently plain static HTML; preorder / email-capture mode.

## Deployment (CI/CD)

Every push to `main` deploys to the live host over FTPS via GitHub Actions
(`.github/workflows/deploy.yml`, using `SamKirkland/FTP-Deploy-Action`). The
deploy is incremental — only changed files are uploaded.

You can also trigger a deploy manually: **Actions** tab → **Deploy to FTP** →
**Run workflow**.

### Required secrets (one-time setup)

Add these under **Settings → Secrets and variables → Actions → New repository secret**:

| Secret         | Value                                            |
| -------------- | ------------------------------------------------ |
| `FTP_SERVER`   | FTP host, e.g. `ftp.evergreendesignbook.com`     |
| `FTP_USERNAME` | FTP username                                     |
| `FTP_PASSWORD` | FTP password                                     |

Credentials live only in GitHub secrets — never commit them.

The site is uploaded to the server's web root (`./`), since the FTP login
lands there directly. If your host uses a different web root (e.g.
`public_html/`), change `server-dir` in the workflow.

### If the connection fails

The workflow uses `protocol: ftps` (encrypted). If your host doesn't support
FTPS and the connection fails, change that line to `protocol: ftp` in
`.github/workflows/deploy.yml`.

## Pre-launch: not yet public

The site is intentionally hidden from search engines while in preview:

- `<meta name="robots" content="noindex, nofollow, ...">` in `index.html`
- `Disallow: /` in `robots.txt`

**Remove both when ready to go public.**

## Switching to 11ty later

The pipeline is ready for an Eleventy migration. Two edits in
`.github/workflows/deploy.yml`:

1. Uncomment the **11ty BUILD STEP** block (Node setup + `npm ci` + build).
2. Change `local-dir: ./` to `local-dir: ./_site/`.

(`node_modules/` and `_site/` are already gitignored.)
