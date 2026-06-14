# Softened Neo-Brutalism Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use `superpowers:subagent-driven-development` (recommended) or `superpowers:executing-plans` to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign the Jekyll research-notes blog with a softened neo-brutalist style, add GitHub and LinkedIn links, and keep it buildable and readable.

**Architecture:** Override Minima theme files (`_includes/header.html`, `_includes/footer.html`, `_includes/custom-head.html`, `_layouts/home.html`) and rewrite the stylesheet in `assets/main.scss`. No new plugins or JavaScript. Verification uses Jekyll build output plus string checks on generated HTML.

**Tech Stack:** Jekyll 4.3, Minima 2.5, SCSS, Liquid, GitHub Pages.

---

## File Structure

| File | Responsibility |
|------|----------------|
| `_includes/custom-head.html` | Loads Sora + Newsreader Google Fonts |
| `_includes/header.html` | Site title + GitHub + LinkedIn icon links |
| `_includes/footer.html` | Copyright + GitHub text link only |
| `_layouts/home.html` | Custom home layout with category/date meta, title, and excerpt cards |
| `assets/main.scss` | All visual styles: palette, typography, layout, components, responsive |
| `index.md` | Updated intro copy for the home page |
| `_config.yml` | Remove unused `minima.social_links` config |

---

### Task 1: Ensure Jekyll environment is ready

**Files:**
- Test: `bundle exec jekyll build`

- [ ] **Step 1: Install dependencies**

Run:
```bash
bundle install
```
Expected: gems installed successfully (may take a moment).

- [ ] **Step 2: Run a baseline build**

Run:
```bash
bundle exec jekyll build
```
Expected: `Build complete.` with no errors.

- [ ] **Step 3: Commit (only if Gemfile.lock changed)**

If `Gemfile.lock` was created or updated:
```bash
git add Gemfile.lock
git commit -m "chore: update bundle dependencies"
```

---

### Task 2: Load Google Fonts

**Files:**
- Create: `_includes/custom-head.html`
- Test: `_site/index.html` contains the font stylesheet link

- [ ] **Step 1: Verify the current site lacks the fonts**

Run:
```bash
bundle exec jekyll build
```
Then:
```bash
grep -q "fonts.googleapis.com" _site/index.html && echo "FAIL: fonts already present" || echo "PASS: fonts missing as expected"
```
Expected: `PASS: fonts missing as expected`.

- [ ] **Step 2: Create the custom head include**

Create `_includes/custom-head.html` with exactly:
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@0,6..72,400;0,6..72,600;1,6..72,400&family=Sora:wght@700;800&display=swap" rel="stylesheet">
```

- [ ] **Step 3: Verify the font link is now present**

Run:
```bash
bundle exec jekyll build
grep -q "fonts.googleapis.com/css2?family=Newsreader" _site/index.html && echo "PASS" || echo "FAIL"
```
Expected: `PASS`.

- [ ] **Step 4: Commit**

```bash
git add _includes/custom-head.html
git commit -m "feat: load Sora and Newsreader fonts"
```

---

### Task 3: Replace site header with social icons

**Files:**
- Create: `_includes/header.html`
- Test: `_site/index.html` contains GitHub + LinkedIn links and no Notes/About nav

- [ ] **Step 1: Verify current header still has text links**

Run:
```bash
bundle exec jekyll build
grep -q 'href="/about"' _site/index.html && echo "About link present" || echo "No About link"
grep -q 'github.com/heruujoko' _site/index.html && echo "GitHub present" || echo "No GitHub"
```
Expected: `About link present` and `No GitHub` (proving we need to change it).

- [ ] **Step 2: Create the custom header**

Create `_includes/header.html` with exactly:
```html
<header class="site-header" role="banner">
  <div class="wrapper">
    <a class="site-title" rel="author" href="{{ "/" | relative_url }}">{{ site.title | escape }}</a>

    <nav class="site-nav social-links" aria-label="Social links">
      <a href="https://github.com/heruujoko" aria-label="GitHub">
        <svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
          <path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/>
        </svg>
      </a>
      <a href="https://www.linkedin.com/in/heruujoko" aria-label="LinkedIn">
        <svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
          <path d="M20.447 20.452h-3.554v-5.569c0-3.328-3.954-3.075-3.954 0v5.569H9.385V8.907h3.414v1.469s.985-1.822 3.481-1.822c3.758 0 4.167 3.452 4.167 5.606v6.292zM5.337 7.433a2.062 2.062 0 1 1 0-4.124 2.062 2.062 0 0 1 0 4.124zm1.777 13.019H3.56V8.907h3.554v11.545zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/>
        </svg>
      </a>
    </nav>
  </div>
</header>
```

- [ ] **Step 3: Verify the new header**

Run:
```bash
bundle exec jekyll build
grep -q 'github.com/heruujoko' _site/index.html && echo "GitHub OK" || echo "FAIL: GitHub missing"
grep -q 'linkedin.com/in/heruujoko' _site/index.html && echo "LinkedIn OK" || echo "FAIL: LinkedIn missing"
grep -q 'href="/about"' _site/index.html && echo "FAIL: About still present" || echo "About removed OK"
grep -q '>Notes<' _site/index.html && echo "FAIL: Notes still present" || echo "Notes removed OK"
```
Expected: GitHub OK, LinkedIn OK, About removed OK, Notes removed OK.

- [ ] **Step 4: Commit**

```bash
git add _includes/header.html
git commit -m "feat: replace header with site title and social icons"
```

---

### Task 4: Replace site footer with GitHub only

**Files:**
- Create: `_includes/footer.html`
- Test: `_site/index.html` footer contains GitHub and no RSS

- [ ] **Step 1: Verify current footer has RSS**

Run:
```bash
bundle exec jekyll build
grep -q 'feed.xml' _site/index.html && echo "RSS present" || echo "No RSS"
```
Expected: `RSS present`.

- [ ] **Step 2: Create the custom footer**

Create `_includes/footer.html` with exactly:
```html
<footer class="site-footer h-card">
  <div class="wrapper">
    <span>&copy; {{ site.time | date: '%Y' }} {{ site.author | default: site.title }}</span>
    <div class="footer-links">
      <a href="https://github.com/heruujoko">GitHub</a>
    </div>
  </div>
</footer>
```

- [ ] **Step 3: Verify the new footer**

Run:
```bash
bundle exec jekyll build
grep -q 'github.com/heruujoko' _site/index.html && echo "GitHub OK" || echo "FAIL"
grep -q 'feed.xml' _site/index.html && echo "FAIL: RSS still present" || echo "RSS removed OK"
```
Expected: GitHub OK, RSS removed OK.

- [ ] **Step 4: Commit**

```bash
git add _includes/footer.html
git commit -m "feat: replace footer with copyright and GitHub link"
```

---

### Task 5: Create custom home layout

**Files:**
- Create: `_layouts/home.html`
- Test: `_site/index.html` has "Latest Notes" heading and post cards with category/date + excerpt

- [ ] **Step 1: Verify current post list has no category labels**

Run:
```bash
bundle exec jekyll build
grep -q 'Latest Notes' _site/index.html && echo "FAIL: already has heading" || echo "Needs custom layout"
```
Expected: `Needs custom layout`.

- [ ] **Step 2: Create the custom home layout**

Create `_layouts/home.html` with exactly:
```html
---
layout: default
---

<div class="home">
  {{ content }}

  {% if site.posts.size > 0 %}
    <h2 class="post-list-heading">{{ page.list_title | default: "Latest Notes" }}</h2>
    <ul class="post-list">
      {% for post in site.posts %}
        <li>
          <p class="post-meta">
            {% if post.categories.size > 0 %}
              {{ post.categories | first | capitalize }} —
            {% endif %}
            {{ post.date | date: "%B %-d, %Y" }}
          </p>
          <h3>
            <a class="post-link" href="{{ post.url | relative_url }}">
              {{ post.title | escape }}
            </a>
          </h3>
          {% if site.show_excerpts %}
            <p class="post-excerpt">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
          {% endif %}
        </li>
      {% endfor %}
    </ul>
  {% endif %}
</div>
```

- [ ] **Step 3: Verify the layout output**

Run:
```bash
bundle exec jekyll build
grep -q 'Latest Notes' _site/index.html && echo "Heading OK" || echo "FAIL: heading missing"
grep -q 'Research' _site/index.html && echo "Category OK" || echo "FAIL: category missing"
grep -q 'post-excerpt' _site/index.html && echo "Excerpt OK" || echo "FAIL: excerpt missing"
```
Expected: Heading OK, Category OK, Excerpt OK.

- [ ] **Step 4: Commit**

```bash
git add _layouts/home.html
git commit -m "feat: custom home layout with category, date, and excerpt cards"
```

---

### Task 6: Update home intro copy

**Files:**
- Modify: `index.md`
- Test: `_site/index.html` contains the new headline

- [ ] **Step 1: Verify current headline**

Run:
```bash
bundle exec jekyll build
grep -q 'Concise notes on papers' _site/index.html && echo "FAIL: new headline already present" || echo "Needs update"
```
Expected: `Needs update`.

- [ ] **Step 2: Replace index.md content**

Replace the entire contents of `index.md` with:
```markdown
---
layout: home
title: ""
list_title: "Latest Notes"
---

<div class="intro">
  <h1>Concise notes on papers, experiments, and ideas in AI.</h1>
  <p class="subtitle">A public research notebook written for humans and agentic collaborators.</p>
</div>
```

- [ ] **Step 3: Verify the new intro**

Run:
```bash
bundle exec jekyll build
grep -q 'Concise notes on papers, experiments, and ideas in AI' _site/index.html && echo "Headline OK" || echo "FAIL"
grep -q 'A public research notebook' _site/index.html && echo "Subtitle OK" || echo "FAIL"
```
Expected: Headline OK, Subtitle OK.

- [ ] **Step 4: Commit**

```bash
git add index.md
git commit -m "feat: update home intro copy"
```

---

### Task 7: Rewrite styles in main.scss

**Files:**
- Modify: `assets/main.scss`
- Test: Build succeeds and generated CSS contains Sora / Newsreader references

- [ ] **Step 1: Verify current styles still use hard neo-brutalism**

Run:
```bash
grep -q 'box-shadow: 6px 6px 0' assets/main.scss && echo "Old styles present" || echo "Already updated"
```
Expected: `Old styles present`.

- [ ] **Step 2: Replace main.scss**

Replace the entire contents of `assets/main.scss` with:
```scss
---
---

@import "minima";

// Softened neo-brutalism palette
$bg:          #FAFAF8;
$text:        #1A1A1A;
$muted:       #6B6B6B;
$border:      #2D2D2D;
$accent:      #0D7377;
$accent-light: rgba(13, 115, 119, 0.08);
$sand:        #F3E9D2;

:root {
  --bg: #{$bg};
  --text: #{$text};
  --muted: #{$muted};
  --border: #{$border};
  --accent: #{$accent};
  --accent-light: #{$accent-light};
  --sand: #{$sand};
}

body {
  background: $bg;
  color: $text;
  font-family: "Newsreader", Georgia, serif;
  font-size: 18px;
  line-height: 1.7;
}

a {
  color: $accent;
  text-decoration: underline;
  text-underline-offset: 3px;
  text-decoration-thickness: 1.5px;
  &:hover {
    background: $accent-light;
    text-decoration: none;
  }
  &:focus {
    outline: 2px solid $accent;
    outline-offset: 2px;
  }
}

.site-header {
  border-bottom: 1px solid $border;
  background: $bg;
  padding: 20px 0;
  position: sticky;
  top: 0;
  z-index: 10;
  min-height: auto;
  .wrapper {
    max-width: 720px;
    margin: 0 auto;
    padding: 0 24px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
}

.site-title, .site-title:visited {
  font-family: "Sora", sans-serif;
  font-weight: 800;
  font-size: 1.25rem;
  letter-spacing: -0.02em;
  color: $text;
  text-decoration: none;
  &:hover {
    background: transparent;
    color: $accent;
  }
}

.site-nav {
  line-height: 1;
}

.social-links {
  display: flex;
  gap: 20px;
  align-items: center;
  a {
    color: $text;
    text-decoration: none;
    display: inline-flex;
    &:hover {
      color: $accent;
      background: transparent;
    }
    &:focus {
      outline: 2px solid $accent;
      outline-offset: 2px;
    }
  }
  svg {
    width: 22px;
    height: 22px;
    vertical-align: middle;
  }
}

.intro {
  margin: 80px 0 64px;
  h1 {
    font-family: "Sora", sans-serif;
    font-weight: 800;
    font-size: 3rem;
    line-height: 1.1;
    letter-spacing: -0.03em;
    margin: 0 0 20px;
  }
  .subtitle {
    font-size: 1.25rem;
    color: $muted;
    margin: 0;
    max-width: 560px;
  }
}

.post-list-heading {
  font-family: "Sora", sans-serif;
  font-weight: 700;
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: $muted;
  margin-bottom: 24px;
  padding-bottom: 12px;
  border-bottom: 1px solid rgba($border, 0.2);
}

.post-list {
  list-style: none;
  padding: 0;
  margin: 0 0 80px;
  > li {
    border: 1px solid $border;
    background: white;
    padding: 28px;
    margin-bottom: 24px;
    border-radius: 4px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.06);
    transition: transform 0.15s ease, box-shadow 0.15s ease;
    &:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 16px rgba(0,0,0,0.08);
    }
  }
  .post-meta {
    font-family: "Sora", sans-serif;
    font-weight: 700;
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.06em;
    color: $accent;
    margin-bottom: 10px;
  }
  h3 {
    font-family: "Sora", sans-serif;
    font-weight: 700;
    font-size: 1.4rem;
    margin: 0 0 10px;
    letter-spacing: -0.01em;
    a {
      color: $text;
      text-decoration: none;
      &:hover {
        color: $accent;
        background: transparent;
      }
    }
  }
}

.post-header {
  margin: 64px 0 48px;
  padding-bottom: 32px;
  border-bottom: 1px solid rgba($border, 0.2);
  .post-title {
    font-family: "Sora", sans-serif;
    font-weight: 800;
    font-size: 2.6rem;
    line-height: 1.1;
    letter-spacing: -0.03em;
    margin: 0;
  }
  .post-meta {
    font-family: "Sora", sans-serif;
    font-weight: 700;
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.06em;
    color: $accent;
    margin-bottom: 16px;
  }
}

.post-content {
  font-size: 1.125rem;
  line-height: 1.8;
  h2 {
    font-family: "Sora", sans-serif;
    font-weight: 700;
    font-size: 1.6rem;
    margin-top: 56px;
    margin-bottom: 20px;
    letter-spacing: -0.02em;
    border-bottom: 2px solid $text;
    padding-bottom: 8px;
  }
  h3 {
    font-family: "Sora", sans-serif;
    font-weight: 700;
    font-size: 1.25rem;
    margin-top: 40px;
    margin-bottom: 16px;
  }
  p {
    margin-bottom: 24px;
  }
  blockquote {
    border-left: 4px solid $accent;
    background: $sand;
    padding: 20px 24px;
    margin: 32px 0;
    font-style: italic;
    color: $text;
    p {
      margin: 0;
    }
  }
  ul, ol {
    margin-bottom: 24px;
    padding-left: 28px;
  }
  li {
    margin-bottom: 8px;
  }
  a {
    color: $accent;
  }
  code {
    font-family: "SF Mono", Monaco, "Cascadia Code", Consolas, monospace;
    background: $accent-light;
    padding: 2px 6px;
    border-radius: 3px;
    font-size: 0.95em;
  }
  pre {
    border: 1px solid $border;
    border-radius: 4px;
    padding: 20px;
    background: white;
    overflow-x: auto;
    box-shadow: 0 2px 8px rgba(0,0,0,0.06);
    margin: 32px 0;
    code {
      background: transparent;
      padding: 0;
    }
  }
}

.post-navigation {
  margin-top: 80px;
  padding-top: 32px;
  border-top: 1px solid $border;
  font-family: "Sora", sans-serif;
  font-size: 0.9rem;
  a {
    color: $accent;
    text-decoration: none;
    font-weight: 700;
    &:hover {
      text-decoration: underline;
      background: transparent;
    }
  }
}

.site-footer {
  border-top: 1px solid $border;
  padding: 40px 0 60px;
  font-size: 0.95rem;
  color: $muted;
  margin-top: 80px;
  .wrapper {
    max-width: 720px;
    margin: 0 auto;
    padding: 0 24px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 16px;
  }
  a {
    color: $text;
    text-decoration: none;
    border-bottom: 1px solid transparent;
    &:hover {
      border-bottom-color: $accent;
      color: $accent;
      background: transparent;
    }
  }
}

@media (max-width: 600px) {
  .intro h1 { font-size: 2.2rem; }
  .post-title { font-size: 1.9rem; }
  .post-list > li { padding: 20px; }
  .site-footer .wrapper { flex-direction: column; align-items: flex-start; }
}
```

- [ ] **Step 3: Verify build and CSS output**

Run:
```bash
bundle exec jekyll build
```
Expected: `Build complete.` with no errors.

Then:
```bash
grep -q "Newsreader" _site/assets/main.css && echo "Newsreader in CSS OK" || echo "FAIL"
grep -q "Sora" _site/assets/main.css && echo "Sora in CSS OK" || echo "FAIL"
```
Expected: both OK.

- [ ] **Step 4: Commit**

```bash
git add assets/main.scss
git commit -m "feat: rewrite styles with softened neo-brutalist palette"
```

---

### Task 8: Clean up unused Minima social config

**Files:**
- Modify: `_config.yml`
- Test: Build still succeeds

- [ ] **Step 1: Remove the unused minima social_links block**

In `_config.yml`, remove the entire `minima:` block so the file ends at `show_excerpts: true`.

Before:
```yaml
show_excerpts: true

minima:
  skin: classic
  social_links: []
```

After:
```yaml
show_excerpts: true
```

- [ ] **Step 2: Verify build still passes**

Run:
```bash
bundle exec jekyll build
```
Expected: `Build complete.` with no errors.

- [ ] **Step 3: Commit**

```bash
git add _config.yml
git commit -m "chore: remove unused minima social_links config"
```

---

### Task 9: Final visual verification

**Files:** none (visual check)
- Test: Home and post pages render as designed

- [ ] **Step 1: Serve the site locally**

Run:
```bash
bundle exec jekyll serve --detach
```
Expected: server starts at `http://127.0.0.1:4000/research-notes/` (or `http://127.0.0.1:4000/` depending on baseurl).

- [ ] **Step 2: Open home page and verify visually**

Open `http://127.0.0.1:4000/research-notes/` in a browser or take a screenshot.

Checklist:
- [ ] Warm off-white background
- [ ] Header shows title + GitHub + LinkedIn icons, no Notes/About
- [ ] Large headline uses Sora, subtitle uses Newsreader
- [ ] "Latest Notes" heading with faint rule
- [ ] Post cards have thin border, subtle shadow, lift on hover
- [ ] Cards show category — date, title, excerpt
- [ ] Footer shows copyright + GitHub only

- [ ] **Step 3: Open a post page and verify visually**

Open `http://127.0.0.1:4000/research-notes/2026/06/13/ai-powered-organization.html` (or equivalent URL) and check:
- [ ] Post title is large Sora
- [ ] Category + date meta is teal
- [ ] Body is Newsreader serif
- [ ] H2 has dark bottom border
- [ ] Blockquote has sand background + teal left border
- [ ] Code/pre blocks styled correctly

- [ ] **Step 4: Stop the server**

Run:
```bash
pkill -f "jekyll serve" || true
```

---

### Task 10: Clean up temporary prototype files

**Files:**
- Delete: `.tmp/prototype-home.html`
- Delete: `.tmp/prototype-post.html`
- Delete: `prototype-home.png`
- Delete: `prototype-home-v2.png`
- Delete: `prototype-post.png`

- [ ] **Step 1: Remove prototypes and screenshots**

Run:
```bash
rm -f .tmp/prototype-home.html .tmp/prototype-post.html prototype-home.png prototype-home-v2.png prototype-post.png
rmdir .tmp 2>/dev/null || true
```

- [ ] **Step 2: Verify no prototype files remain**

Run:
```bash
ls .tmp 2>/dev/null || echo "No .tmp directory"
ls prototype-*.png 2>/dev/null || echo "No prototype screenshots"
```
Expected: both report nothing remaining.

- [ ] **Step 3: Commit cleanup**

```bash
git add -A
git commit -m "chore: remove temporary redesign prototypes"
```

---

## Self-Review

**Spec coverage check:**
- Warm off-white background, charcoal text, teal accent → Task 7
- Sora + Newsreader fonts → Task 2 + Task 7
- Header: title + GitHub + LinkedIn icons → Task 3
- Home intro + Latest Notes + post cards → Task 5 + Task 6
- Post page styling → Task 7
- Footer: copyright + GitHub only → Task 4
- No RSS → Task 4
- Responsive behavior → Task 7 media query
- Accessibility: aria-labels, focus outlines → Task 2, Task 7

**Placeholder scan:** No TBD/TODO/fill-in details found.

**Type consistency:** All class names (`social-links`, `post-list-heading`, `post-excerpt`, `post-meta`, `post-navigation`) match between HTML and SCSS.

**Execution handoff:** Plan is ready. Use `superpowers:subagent-driven-development` (recommended) or `superpowers:executing-plans` to implement.