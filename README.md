# My Blog — Setup Guide

A simple blog hosted free on GitHub Pages. No coding needed to publish posts.

## One-time setup (about 5 minutes)

1. **Create a GitHub account** at github.com if you don't have one.
2. **Create a new repository:** click the **+** in the top-right → **New repository**.
   - Name it exactly `YOURUSERNAME.github.io` (replace with your actual username). This makes your blog live at that address.
   - Keep it **Public**, then click **Create repository**.
3. **Upload these files:** on the repository page, click **uploading an existing file** (or **Add file → Upload files**). Drag in **everything from this folder** — including the `_layouts`, `_posts`, and `assets` folders. Click **Commit changes**.
   - Tip: if drag-and-drop doesn't preserve folders, drag the folders themselves from your file explorer, not just the files inside them.
4. **Turn on GitHub Pages:** go to **Settings → Pages** (left sidebar). Under "Build and deployment", set Source to **Deploy from a branch**, pick branch **main** and folder **/ (root)**, then **Save**.
5. Wait 1–2 minutes, then visit `https://YOURUSERNAME.github.io` — your blog is live.

## Make it yours

Open `_config.yml` in the repository, click the **pencil icon** to edit, and change the top three lines:

```yaml
title: My Blog
description: Notes, essays, and things worth writing down.
author: Your Name
```

Commit the change and the site updates automatically.

## How to publish a new post

1. In your repository, open the `_posts` folder.
2. Click **Add file → Create new file**.
3. Name it with the date first: `2026-07-20-my-first-real-post.md`
   (format must be `YYYY-MM-DD-title-with-dashes.md`)
4. Paste this at the top, then write your post below it in plain text or Markdown:

```
---
layout: post
title: "My First Real Post"
---

Your writing goes here.
```

5. Click **Commit changes**. Your post appears on the site within a minute or two.

You can also write posts on your phone or computer in any text editor and upload the `.md` file via **Add file → Upload files**.

## Adding images to posts

1. Upload the image into the `assets` folder (Add file → Upload files).
2. In your post, add: `![description](/assets/your-image.jpg)`

## Editing or deleting posts

Open the file in `_posts`, click the **pencil icon** to edit or the **trash icon** to delete, and commit.
