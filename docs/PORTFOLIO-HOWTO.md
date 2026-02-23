# Portfolio Site — Contributor Reference Guide

This guide covers how to maintain and extend the Jekyll-based portfolio at
`https://jfwalasik.github.io/portfolio`. It documents the patterns already in
use so that any collection, page, or featured item can be added or updated
consistently.

---

## 1. Site Structure Overview

The site is built with [Jekyll](https://jekyllrb.com/) on the
[Hitchens theme](https://github.com/patdryburgh/hitchens), hosted via GitHub
Pages. It was originally a blogging theme and has been extended with Jekyll
**collections** to organize portfolio work by category.

### Key files and folders

| Path | Purpose |
|---|---|
| `_config.yml` | Site-wide settings; all collections must be declared here |
| `index.html` | Homepage: featured work grid + "All Collections" list |
| `_data/featured.yml` | Controls which items appear in the featured grid |
| `assets/images/` | General images referenced in content |
| `assets/images/featured/` | Thumbnail images for the featured grid |
| `_layouts/` | Jekyll layout templates (`post`, `fullwidth`, `collection`, etc.) |

### The dual-folder pattern

Every collection has **two** corresponding locations in the repo:

| Folder | What it contains |
|---|---|
| `_collection_name/` | The actual content files (markdown or HTML) |
| `collection-name/` | The collection's index page (`index.html`) that lists all items |

For example, the Security Training collection has:
- `_security_training/` — content files
- `security-training/index.html` — the listing page visitors see

---

## 2. Adding a New Collection

Adding a collection requires changes in four places. Complete them in order.

### Step 1 — Declare the collection in `_config.yml`

Open `_config.yml` and add an entry under `collections:`. Follow the existing
pattern exactly, using underscores for the key name:

```yaml
collections:
  your_collection_name:
    output: true
    title: "Your Collection Display Name"
    permalink: /your-collection-name/:name/
```

Then add a default layout for it under `defaults:`:

```yaml
defaults:
  - scope:
      type: "your_collection_name"
    values:
      layout: "post"
```

Use `layout: "post"` for standard markdown content. Use `layout: "fullwidth"`
for pages with custom HTML that should render without the theme's sidebar or
content-width restrictions.

**Note:** Jekyll does not reload `_config.yml` automatically during local
development. Restart `bundle exec jekyll serve` after editing it.

### Step 2 — Create the content folder

Create a new folder in the repo root named with a leading underscore and
underscores between words:

```
_your_collection_name/
```

This is where your content files will live (see Section 3).

### Step 3 — Create the collection index page

Create a folder named with hyphens (no leading underscore) and add an
`index.html` inside it:

```
your-collection-name/
  index.html
```

Use this template for the index page:

```html
---
layout: collection
title: Your Collection Display Name
---
<ul class="post-list">
  {% for item in site.your_collection_name %}
  <li>
    <a href="{{ item.url | relative_url }}">
      <h3>{{ item.title }}</h3>
      {% if item.description %}
      <h6>{{ item.description }}</h6>
      {% endif %}
    </a>
  </li>
  {% endfor %}
</ul>
```

Replace `your_collection_name` (with underscores) to match the key in
`_config.yml`.

### Step 4 — Add the collection to the homepage

Open `index.html` and add a new list item inside the `collections-section`
block. Follow the existing pattern:

```html
{% if site.your_collection_name.size > 0 %}
<li>
  <a href="{{ '/your-collection-name/' | relative_url }}">
    <h3>Your Collection Display Name</h3>
    <p>{{ site.your_collection_name.size }} item{% if site.your_collection_name.size != 1 %}s{% endif %}</p>
  </a>
</li>
{% endif %}
```

The `{% if ... size > 0 %}` guard means the collection will not appear on the
homepage until it has at least one content file.

### Step 5 (optional) — Add a featured item

See Section 5 for instructions on adding items to the featured grid.

---

## 3. Adding Content to a Collection

### Markdown content (most common)

Create a `.md` file inside the collection's underscore folder. The filename
becomes the URL slug, so use lowercase with hyphens:

```
_your_collection_name/your-item-title.md
```

Every markdown file must begin with a front matter block:

```yaml
---
title: Your Item Title
description: A one-sentence description shown in listings and featured cards
date: 2025-01-01
---
```

`date` controls sort order within the collection. `description` is optional
but recommended — it appears under the title in the collection index.

After the front matter, write standard Markdown:

```markdown
## Section Heading

Body text goes here. You can use all standard Markdown formatting.

![Alt text for the image](/portfolio/assets/images/your-image.png)

[Link text](https://example.com)
```

**Image paths:** Images must be uploaded to `assets/images/` in the repo.
Reference them with the full path starting from `/portfolio/`:

```markdown
![Description](/portfolio/assets/images/your-image.png)
```

Alternatively, use the Liquid filter for more portable links:

```markdown
![Description]({{ '/assets/images/your-image.png' | relative_url }})
```

Both work; the hardcoded `/portfolio/` path is simpler but would need updating
if the site's `baseurl` ever changes.

### Linking to a GitHub repository

Use standard Markdown link syntax:

```markdown
[View the code on GitHub](https://github.com/yourusername/your-repo)
```

### Crediting a source

For tools or code you forked or built upon, include a citation in plain text
or a blockquote at the bottom of the file:

```markdown
Source: Liu, Chen. "CitationMap: A Python Tool to Identify and Visualize Your
Google Scholar Citations Around the World." Authorea Preprints (2024).

[Original code on GitHub](https://github.com/ChenLiu-1996/CitationMap)
```

---

## 4. Embedding Google Drive Content

Google Drive files (PDFs, PowerPoint slide decks, Google Forms exports) can be
embedded as inline previews using an `<iframe>`. This is the pattern used in
the Security Training collection.

### Getting the correct Drive URL

1. Open the file in Google Drive.
2. Click **Share** and set access to **Anyone with the link** (Viewer).
3. Copy the share link. It will look like:
   `https://drive.google.com/file/d/FILE_ID/view?usp=sharing`
4. Change `/view?usp=sharing` to `/preview` to get the embeddable URL:
   `https://drive.google.com/file/d/FILE_ID/preview`

### Embedding in a content file

Use the `<iframe>` tag directly in your markdown file (Jekyll passes raw HTML
through):

```html
<iframe
  src="https://drive.google.com/file/d/YOUR_FILE_ID/preview"
  width="100%"
  height="700px">
</iframe>
```

Adjust `height` as needed. `700px` works well for slide decks and
single-page PDFs. Add `<br><br>` between multiple iframes for spacing.

**Full example** (from the Security Training collection):

```markdown
---
layout: pdf
title: Security Awareness Training
description: Employee security training presentation and quiz
date: 2024-07-15
back_link: /security-training/
---

## Security Training and Quiz

Presentation in Powerpoint; 10-Question Quiz in Google Forms.

<br>
<br>

<iframe src="https://drive.google.com/file/d/1CxZRIYFRyY_Eamu-a_Xtdvwu1iELYLbz/preview" width="100%" height="700px"></iframe>

<br>
<br>

<iframe src="https://drive.google.com/file/d/1BK1nCR4QH6zR4csZNcr5iZCmVA90Q1FC/preview" width="100%" height="700px"></iframe>
```

**Supported file types for Drive embedding:** PDF, PowerPoint (.pptx), Word
(.docx), Google Slides, Google Docs, Google Forms (as PDF export). Native
Google files embed most reliably. Uploaded Office files may show a simplified
preview.

**Note on the `back_link` field:** This front matter field appears in some
content files and adds a navigation link back to the collection index. It is
not required but is a useful UX touch for long or embedded content.

---

## 5. Adding HTML or Interactive Content

For pages with custom layouts, JavaScript interactions, or any content that
should not be wrapped in the theme's default content-width styles, create an
`.html` file (rather than `.md`) directly in the collection's underscore folder.

### Structure

The file starts with a Jekyll front matter block, followed by the full HTML
content. Jekyll processes the front matter and wraps the file in the specified
layout; everything below the front matter block is rendered as-is.

```html
---
layout: fullwidth
title: Your Page Title
description: A short description shown in collection listings
date: 2025-01-01
---

<!-- Your full HTML starts here -->
<style>
  /* Custom styles */
</style>

<div class="your-content">
  <!-- Content and JavaScript -->
</div>

<script>
  // Your JavaScript
</script>
```

### When to use `fullwidth` vs `post`

Use `layout: fullwidth` when:
- The page has its own `<style>` block that controls layout
- The content uses the full browser width (e.g., timelines, maps, interactive
  tools)
- The theme's default content width would constrain the design

Use `layout: post` (or omit `layout` if set as a collection default in
`_config.yml`) for standard text and image content.

### Example: The Premillennial Dispensationalism interactive timeline

This page (`_i_believe/Premillenial-Dispensationalism.html`) is a
self-contained interactive HTML document. Its front matter:

```yaml
---
layout: fullwidth
title: Premillennial Dispensationalism - an Interactive Timeline (click tiles for more detail)
description: A visual guide and reference developed with Claude (AI)
date: 2025-05-29
---
```

Everything below the front matter is standard HTML — styles, markup, and
JavaScript — all in a single file. Jekyll serves it as a page within the
`i_believe` collection without any additional configuration beyond what is
already in `_config.yml`.

### Tips for HTML content files

- Keep all styles in a `<style>` block in the same file unless they are very
  large, in which case consider a separate CSS file in `assets/`.
- JavaScript can live in a `<script>` block at the bottom of the file.
- Avoid referencing external resources that may become unavailable. Prefer
  CDN-hosted libraries with stable URLs when external scripts are needed.
- Test locally with `bundle exec jekyll serve` before pushing, as HTML errors
  will not be flagged by Jekyll — the page will simply render incorrectly.

---

## 6. Updating a Collection Title

A collection's display title can appear in up to four places. Update all of
them to keep the site consistent.

| Location | What to change |
|---|---|
| `_config.yml` | The `title:` field under the collection's entry |
| `index.html` (homepage) | The `<h3>` text inside the collection's `<li>` block |
| `collection-name/index.html` | The `title:` in the front matter |
| `_data/featured.yml` | Any featured item entries that reference this collection (description text only — the URL does not change) |

**Example:** To rename "I Believe" to "Personal Writing":

In `_config.yml`:
```yaml
  i_believe:
    output: true
    title: "Personal Writing"   # changed
    permalink: /i-believe/:name/
```

In `index.html`:
```html
<h3>Personal Writing</h3>   <!-- changed -->
```

In `i-believe/index.html` front matter:
```yaml
---
layout: collection
title: Personal Writing   # changed
---
```

**Note:** The folder names (`_i_believe/` and `i-believe/`) and the permalink
do not need to change when you rename the display title. Changing the permalink
would break existing links; only do so if you also set up redirects.

---

## 7. Adding to the Featured Section

The featured grid on the homepage is driven by `_data/featured.yml`. Each
entry in this file creates one card in the grid.

### `_data/featured.yml` format

```yaml
- title: Your Item Title
  description: One sentence shown under the title on the card
  image: /assets/images/featured/your-thumbnail.png
  url: /your-collection-name/your-item-slug/
```

All four fields are used:

| Field | Notes |
|---|---|
| `title` | Displayed as the card heading |
| `description` | Short caption below the title |
| `image` | Path to thumbnail image in `assets/images/featured/` |
| `url` | The URL of the collection item or collection index page |

### Thumbnail images

- Place the image in `assets/images/featured/`
- Recommended size: at least **600 × 360px** (the grid renders images at
  `height: 180px` with `object-fit: cover`, so wider images crop gracefully)
- Use `.png` or `.jpg`
- Name the file clearly to match the item (e.g., `citation-map-o1.png`)

### Grid layout

The featured grid uses three columns on wide screens, two columns on medium
screens (≤ 900px), and one column on mobile (≤ 600px). Items appear in the
order they are listed in `featured.yml`. To reorder featured items, simply
reorder the entries in the file.

---

## Quick Reference: Content File Checklist

When adding any new content file, confirm:

- [ ] Front matter block is present and starts on line 1
- [ ] `title` is set (required for collection listings)
- [ ] `date` is set (controls sort order)
- [ ] `description` is set if the item will appear in featured grid or collection index
- [ ] Images are uploaded to `assets/images/` and paths are correct
- [ ] Google Drive files are shared as "Anyone with the link" and use `/preview` URL
- [ ] If HTML file: `layout: fullwidth` is set in front matter
- [ ] If new collection: all four steps in Section 2 are complete
