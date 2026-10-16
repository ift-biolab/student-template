# Student documentation

> You normally only need to edit files inside `content/`. The remaining files configure and publish the website.

> This repository and its published site are public. Do not commit passwords, personal data, confidential partner information, unpublished proprietary protocols or restricted research data.

Write your own content in English or French. The interface stays in English. All example content is placeholder material.

## Edit a page

1. On github.com, open a file such as `content/project/overview/index.md` and click the pencil (**Edit this file**).
2. Keep the opening block between `---` lines. Replace the prompts below it.
3. Use **Preview** and review the diff under **Changes**. Click **Commit changes**, enter a short description, and commit to `main` (or use your administrator's pull request process).
4. Open **Actions → Build and deploy Pages**. After the build and deploy jobs turn green, reload your site. The first publication may take a few minutes.

## Add an experiment or week

Copy the whole `content/experiments/experiment-01/` folder to `content/experiments/experiment-02/`, or `content/notebook/week-02/` to `content/notebook/week-03/`.

On github.com, where there is no folder-copy button: open the example `index.md`, copy its text, then use **Add file → Create new file** in the repository. Enter the full new path, for example `content/notebook/week-03/index.md`, and paste the text. Copy/upload any images into the same new folder too.

Change only these values at the top, keeping the `---` lines:

```yaml
---
title: "Week 03"
weight: 30
---
```

Use `Experiment 02` and `weight: 20` for the second experiment. Higher weights appear later within the same section. Replace the prompts and commit. The menu updates automatically. Add a Markdown link to the section's `_index.md` if you also want it in that page's written list.

## Add an image

Open the page's folder, choose **Add file → Upload files**, upload and commit the image. Reference its exact filename:

```markdown
![Describe what this image shows](my-image.webp)
```

Use lowercase ASCII names, hyphens and no spaces. Compress photographs as JPEG or WebP; use SVG or PNG for diagrams. Link externally to videos. Do not commit raw camera dumps, raw datasets or very large binary files without agreement. Git LFS is not used.

## Markdown essentials

````markdown
## Heading

A paragraph with **bold** and *italic* text.

[Project overview](../../project/overview/index.md)
![Descriptive alt text](my-image.webp)

1. First item
2. Second item

| Item | Notes |
|---|---|
| To be completed | To be completed |

```text
Plain-text notes or code; add only approved procedures.
```

## References

- Author. Year. *Title*. Publisher or journal. DOI or URL.
````

Links to another content file can use its relative `.md` path. Relearn resolves them to published pages. Keep images alongside `index.md`.

## If the site does not update

Open **Actions**, select the latest failed run, then the **Build** step. Note any filename and line number. Check your last edit, especially the opening `---` block and image filenames. Contact the lab administrator if unsure. Ask before changing files outside `content/`; you do not need Hugo installed.

## Administrator reference

See [ADMIN.md](ADMIN.md) for local preview, deployment, recovery and future branding.

Pinned toolchain: **Hugo 0.168.0** (standard), **Relearn 9.2.1** (exact release commit in `go.mod`/`go.sum`), **Go 1.27.2**. Relearn requires Hugo >= 0.166.0. Official Pages actions: configure-pages **6.0.0**, upload-pages-artifact **5.0.0**, deploy-pages **5.0.1**; checkout **7.0.1**, setup-go **7.0.0**. All action references are pinned to release commit hashes.

Sources checked on 2026-10-16: [Relearn requirements](https://mcshelby.github.io/hugo-theme-relearn/introduction/quickstart/), [Relearn release](https://github.com/McShelby/hugo-theme-relearn/releases/tag/9.2.1), [Hugo release](https://github.com/gohugoio/hugo/releases/tag/v0.168.0), [Hugo Pages workflow](https://gohugo.io/host-and-deploy/deploy-to-github-pages/), [GitHub Pages workflows](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages).
