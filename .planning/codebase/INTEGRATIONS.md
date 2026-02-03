# External Integrations

**Analysis Date:** 2026-02-03

## APIs & External Services

**GitHub:**
- Service: GitHub API
  - Purpose: Branch PR detection, content management
  - SDK/Client: GitHub CLI (gh) in CI/CD
  - Auth: GitHub Actions token (`GITHUB_TOKEN`)
  - Integration point: `.github/workflows/deploy-hugo.yaml`

**GitHub Module Registry:**
- Service: GitHub module hosting (for Hugo modules)
  - Purpose: Theme distribution
  - Module: `github.com/temporalinterference/z43-cards-theme`
  - Auth: Not required (public repository)

## Data Storage

**Databases:**
- Not applicable - Static site generator, no database

**File Storage:**
- Local filesystem only
  - Content: `content/` directory
  - Assets: `assets/` directory
  - Generated: `resources/` directory (build artifacts)
  - Public output: `public/` directory

**Caching:**
- None configured - Static assets cached by GitHub Pages HTTP headers

## Authentication & Identity

**Auth Provider:**
- GitHub Actions built-in authentication
  - Method: GITHUB_TOKEN (automatic)
  - Scope: Actions, Pages, Pull Requests (defined in workflow)
  - Usage: PR comments, branch detection, artifact uploads

**Configuration:**
- No user authentication required (static site)
- No external identity providers

## Monitoring & Observability

**Error Tracking:**
- Not configured - Static site has no runtime errors
- Build failures: Visible in GitHub Actions workflow runs

**Logs:**
- GitHub Actions logs: Available in workflow runs
- Hugo build output: Captured in CI/CD pipeline
- No external logging service

## CI/CD & Deployment

**Hosting:**
- GitHub Pages (automatic)
  - Primary: Production site at https://itis-usa.org
  - Preview: Branch-specific previews at `https://[domain]/branch-[name]/`

**CI Pipeline:**
- GitHub Actions workflow: `.github/workflows/deploy-hugo.yaml`
  - Triggers:
    - Push to any branch
    - Pull request (opened, synchronize, reopened)
  - Jobs:
    1. check-pr: Detects if branch has associated PR
    2. build: Builds site with Hugo, security checks
    3. deploy: Deploys to GitHub Pages
  - Concurrency: One build per branch, cancels previous if new push detected

**Build Process:**
- Tool versions installed via `mise` action
- Dart Sass downloaded separately for GitHub Actions
- Hugo modules initialized with `hugo mod get -u`
- Theme init script runs: `init-theme.sh`
- Hugo build: `hugo --minify` with dynamic baseURL

**Security:**
- Fork PR verification checks:
  - Suspicious file detection (.php, .cgi, .sh, .exe, .dll)
  - Path whitelist enforcement (content/, static/, layouts/, data/, i18n/, assets/)
  - Repository size limit (500MB)
  - Hugo config validation
  - Large file detection (>10MB)
  - Binary executable detection
  - Content directory validation
- Configuration change monitoring
- PR comments with deployment links

## Environment Configuration

**Required env vars:**
- `GITHUB_TOKEN` (automatic in Actions) - For GitHub API access
- `GH_TOKEN` (in check-pr job) - For PR detection
- `HUGO_ENVIRONMENT=production` (in build job)
- `HUGO_ENV=production` (in build job)

**Build-time vars:**
- `DART_SASS_VERSION: 1.93.2` - Explicit version for reproducibility
- `baseURL` - Dynamically set per deployment:
  - Main branch: `https://itis-usa.org/`
  - Branch previews: `https://itis-usa.org/branch-[branch-name]/`

**Secrets location:**
- GitHub Actions Secrets (accessed via `${{ secrets.GITHUB_TOKEN }}`)
- No custom secrets required for current implementation

## Webhooks & Callbacks

**Incoming:**
- GitHub webhook for branch pushes: Automatic, no configuration needed
- GitHub webhook for PRs: Automatic, no configuration needed

**Outgoing:**
- PR Comments: Automatic via peter-evans/create-or-update-comment action
  - Posts preview URL and branch links
  - Comment format: Markdown with deployment information

## Artifact Management

**Build Artifacts:**
- Stored via `actions/upload-pages-artifact@v3`
  - Path: `./current-site` (complete site directory)
  - Retention: 7 days
  - Purpose: Site persistence across deployments

**Previous Site Download:**
- Downloaded via `dawidd6/action-download-artifact@v6`
  - Workflow: deploy-hugo.yaml
  - Artifact name: github-pages
  - Purpose: Preserve branch preview directories during main branch builds

**Site Preservation:**
- Branch previews merged into main deployment
- `robots.txt` generated to prevent indexing of branch- paths
- `previews.html` generated as index of all available previews

## Deployment Configuration

**GitHub Pages Settings:**
- Deployment trigger: Automatic via `actions/deploy-pages@v4`
- Environment: `github-pages` (GitHub Actions environment)
- URL source: Dynamic from Pages configuration
- Permissions required: contents:read, pages:write, id-token:write

**Branch Strategy:**
- Main branch: Generates root site at base URL
- Other branches: Generates previews at `/branch-[name]/`
- PR handling: Same as feature branch
- PR skip logic: Skips build if branch already has open PR and not a pull request event

---

*Integration audit: 2026-02-03*
