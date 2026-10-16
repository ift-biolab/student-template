# Administrator runbook

## Scope and ownership

IFT BioLab uses the confirmed GitHub organization slug `ift-biolab`. V1 repositories and sites are public. Students edit Markdown and images in `content/`; this runbook is for administrators. No credentials or custom secrets are required by the workflow.

Select the existing `ift-biolab` organization, or have its intended owner create it manually if it does not exist. Confirm ownership and availability before deployment. Keep repositories organization-owned. The administrator must be an organization **Owner**, which gives Admin access to its repositories. Prefer a second trusted Owner for continuity. Set organization base repository access to **No permission**; grant access per repository.

Students receive repository **Write**, which allows content commits, but not **Admin**, which controls settings and access. An **outside collaborator** can have Write on just their repository without organization membership. Do not share accounts.

## Initial publication of these local directories (manual; not executed)

1. In `ift-biolab`, create two empty **public** repositories: `student-template` and `2027-demo-student`. Do not initialize them with a README, license or `.gitignore`.
2. Copy the complete contents of the corresponding local directory, including `.github/`, `.gitignore`, `go.mod` and `go.sum`. Do not publish the parent handoff directory, `.local-tools/`, `public/` or `resources/`. For a browser upload, ensure hidden files are included; uploading through Git preserves them reliably.
3. From `student-template`, after authorizing publication, run the following (commands are instructions only):

   ```powershell
   git init -b main
   git add .
   git commit -m "Initial student documentation site"
   git remote add origin https://github.com/ift-biolab/student-template.git
   git push -u origin main
   ```

   Separately, from `student-demo`:

   ```powershell
   git init -b main
   git add .
   git commit -m "Initial demo student documentation site"
   git remote add origin https://github.com/ift-biolab/2027-demo-student.git
   git push -u origin main
   ```

4. In **student-template → Settings → General**, enable **Template repository**. Keep `2027-demo-student` a normal repository.
5. In each repository, choose **Settings → Pages → Build and deployment → Source → GitHub Actions**. Ensure organization policy allows the official actions used in `.github/workflows/pages.yml`. No DNS or custom domain is configured.
6. Open **Actions → Build and deploy Pages → Run workflow → main** if the initial push ran before Pages was enabled. Confirm the `github-pages` environment permits `main` deployments. If environment protection requires review, its designated reviewer approves the deployment.
7. Confirm both build and deploy jobs are green. Follow the deployment URL. Expected URLs: `https://ift-biolab.github.io/student-template/` and `https://ift-biolab.github.io/2027-demo-student/`. Test sidebar navigation, search for `hypothesis`, and the experiment image.
8. In the demo, edit one Markdown sentence via GitHub, commit it, wait for another green deployment, and confirm the published sentence changes. This final check requires GitHub access and has not been performed locally.

## Create a future student repository

1. Open `ift-biolab/student-template` and choose **Use this template → Create a new repository**.
2. Select owner `ift-biolab`, visibility **Public**, and a name such as `<year>-<student-slug>` or `<year>-<project-slug>`.
3. Under **Settings → Collaborators and teams / Manage access**, invite the student with **Write** access on this repository only; verify acceptance. Keep the template administrator-maintained.
4. Configure **Pages → Source → GitHub Actions** and run the workflow on `main`.
5. Update `title` and optionally the fallback `baseURL` in `hugo.toml` for local production builds; the workflow gets the real URL from `configure-pages` and overrides the fallback automatically. The local `go.mod` module name is only an identifier, so a template copy can retain it.
6. After green deployment, give the student the repository link, Pages link and README.

## Local build and review (administrator only)

Install standard Hugo **0.168.0**, Go **1.27.2**, and Git on your PATH. No Node, Sass or student-side local installation is needed. In this repository:

```powershell
hugo version
go version
hugo mod graph
hugo mod verify
hugo --gc --minify --panicOnWarning
hugo server --bind 127.0.0.1 --baseURL http://localhost:1313/2027-demo-student/ --port 1313 --disableFastRender
```

Open `http://localhost:1313/2027-demo-student/`. Keep the terminal running and stop with Ctrl+C. This is a project-site preview, including the subpath. To check another repository name, change the path in `--baseURL`. To check root-site compatibility, build with `hugo --baseURL https://ift-biolab.github.io/`; do not create the main site during Phase 1.

The parent handoff's `tools/bootstrap.ps1` supplies verified local tools for review without a machine-wide installation; see its Phase 1 report. Generated `public/`, `resources/` and build locks are ignored and must not be committed.

## Repair a broken site

1. Open the repository **Actions** tab and the failed run. Note its commit ID and compare it with the last green run.
2. Inspect **Build**, or the earlier failing step. Hugo reports the filename and often the line. Open that file and its **History** / commit diff.
3. Check malformed YAML front matter, a missing closing `---`, an invalid image path, spaces or unusual characters in filenames, a deleted `_index.md`, or accidental edits to `hugo.toml`, `go.mod`, `go.sum` or `.github/workflows/pages.yml`. Image and content-link errors are configured to fail the build.
4. For a small repair, open the file on GitHub, click the pencil, correct it, preview the diff and commit to `main`. You can do this with your own administrator account.
5. To undo a merged pull request in the web UI, open **Pull requests → merged PR → Revert**, create the generated reversal PR, review it and merge it. GitHub does not provide a general Revert button for every direct commit. For a direct content commit, view the file at the previous good commit, copy its contents, and replace the current file through the editor; restore each affected file (and delete newly introduced files) for a complete reversal.
6. For a deleted file, open repository history, find its deletion commit and browse the parent commit. Copy the old file contents, use **Add file → Create new file** with the original path, and commit. For deleted images, download the old image from that revision and upload it at the original path.
7. Command-line alternative, from a clone with a clean working tree (review before pushing):

   ```powershell
   git pull --ff-only
   git show <bad-commit>
   git revert <bad-commit>
   git push
   ```

   A merge commit needs a correctly chosen mainline parent; ask an experienced maintainer rather than guessing. Avoid force pushes and history resets.
8. Confirm the repair has a green build **and** deploy run, open its `github-pages` URL, reload and inspect the affected page and image. A failed build leaves the last successful site live; green build alone does not prove deployment succeeded.

## Branding later

Use `hugo.toml` for the site `title` and standard Relearn `params.themeVariant` (currently neutral `zen-light`). No logo, custom colors or typography have been invented.

- Optional logo: place an approved file at `assets/images/logo.svg`. Relearn detects it automatically. To choose another file, set `logo = { src = '/images/approved-logo.svg' }` inside `[params]`; Relearn resolves this asset path with the project subpath.
- Optional favicon: place it at `assets/images/favicon.png` (or `.svg` / `.ico`). Relearn detects the standard filename automatically; no layout override is needed.
- Future colors: create `assets/css/theme-ift-biolab.css` based on Relearn's variant CSS variables, then set `[params] themeVariant = 'ift-biolab'`.
- Future additional CSS: use Relearn's `layouts/partials/custom-header.html` hook only when needed, obtaining its URL with Hugo's `relURL`. No override is required in V1.

See the [Relearn configuration reference](https://mcshelby.github.io/hugo-theme-relearn/configuration/reference/). Add these keys inside the existing `[params]` table, before its nested `[params.image]` and `[params.link]` tables. Retest project subpaths after branding changes.

## Updates and publication limits

GitHub template copies have independent histories. Later changes to `student-template` do **not** automatically propagate to existing repositories. The pinned Hugo module avoids copying theme code into each repository; updating that pin or configuration still requires patches to active repositories. Test upgrades in the demo, then patch active repositories manually in V1. No batch updater is included.

Keep the Hugo and Go versions consistent between this guide, README and workflow. Relearn **9.2.1** is locked to its exact release commit in `go.mod`, with checksums in `go.sum`. Re-run local acceptance checks before changing any version or action hash.

Repositories and Pages sites are public. Never upload sensitive data. Private publishing and integration with `ift.devinci.fr` are out of scope. No project license has been selected, so no LICENSE file is supplied; decide licensing before granting reuse rights. The upstream theme retains its own license in the module cache.
