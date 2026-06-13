# Blog Redesign: Softened Neo-Brutalism

## Summary

Redesign the Jekyll research-notes blog from hard neo-brutalism to a softer, structured, editorial neo-brutalist look inspired by MongoDB Careers. Keep the blog’s character (bold borders, blocky cards, high-contrast accents) but refine it with a muted palette, distinctive typography, generous whitespace, and GitHub + LinkedIn links.

## Goals

- Soften the existing visual style without losing its neo-brutalist identity.
- Add persistent links to the author’s GitHub and LinkedIn profiles.
- Improve readability for long-form research notes.
- Keep the implementation simple and maintainable within Jekyll + Minima.

## Non-Goals

- No new pages or features beyond the redesign and social links.
- No JavaScript-driven interactivity (CSS-only hover/focus states).
- No dark mode in this version.

## References

- User reference: https://www.mongodb.com/company/careers — clean, structured, confident, generous whitespace.
- Author GitHub: https://github.com/heruujoko
- Author LinkedIn: https://www.linkedin.com/in/heruujoko

## Visual Design

### Color Palette

| Token | Value | Usage |
|-------|-------|-------|
| Background | `#FAFAF8` | Page background, warm off-white |
| Text | `#1A1A1A` | Headings, body text, primary borders |
| Muted text | `#6B6B6B` | Subtitles, excerpts, footer text |
| Border | `#2D2D2D` | Thin dark borders (1–2px) |
| Accent | `#0D7377` | Meta labels, links, hover states, blockquote border |
| Accent light | `rgba(13, 115, 119, 0.08)` | Inline code background, link hover background |
| Sand | `#F3E9D2` | Blockquote background |
| Shadow | `0 2px 8px rgba(0,0,0,0.06)` | Subtle card shadows |

### Typography

- **Headings**: Sora (Google Fonts), weights 700 and 800. Modern geometric sans with character.
- **Body**: Newsreader (Google Fonts), weight 400 and 600. High-contrast editorial serif for comfortable reading.
- **Scale**: body 18px / line-height 1.7; home h1 3rem; post title 2.6rem; card title 1.4rem.
- **Code**: system monospace stack, teal-tinted background.

### Layout

- Max content width: 720px, centered.
- Generous vertical spacing: 64–80px between major sections.
- Sticky header with 1px bottom border.
- Cards with 1px border, 4px border-radius, subtle shadow, slight lift on hover.

## Components

### Site Header

- Left: site title “AI Research Notes” as a home link.
- Right: GitHub and LinkedIn icons only (no text nav links).
- Icons are 22px, currentColor, hover turns accent teal.
- Sticky on scroll, solid warm off-white background.

### Home Page

- Intro block: large Sora headline + Newsreader subtitle.
- “Latest Notes” section heading: uppercase, small, muted, with faint bottom rule.
- Post list: cards showing category/date, title, excerpt.
- Each card links to the post.

### Post Page

- Post header: category + date meta, large title.
- Post content: serif body, Sora subheadings.
- H2 has a 2px dark bottom border (the one retained “brutalist” detail).
- Blockquote: sand background, 4px left accent border.
- Inline code: accent-light background, rounded 3px.
- Pre blocks: white background, 1px border, subtle shadow.
- Footer nav: “← Back to all notes” link in accent teal.

### Site Footer

- Left: copyright “© 2026 Heru Joko”.
- Right: GitHub text link only.
- No RSS link.
- 1px top border, muted text.

## Interactions

- Nav icons: color change to accent on hover.
- Post cards: translateY(-2px) + deeper shadow on hover.
- Links in body: underline by default; hover removes underline and adds accent-light background.
- Focus states: 2px outline in accent color for keyboard users.

## Responsive Behavior

- Same single-column layout at all widths.
- On small screens (≤600px): reduce headline sizes and card padding.
- Header remains horizontal; icons stay on the right.

## Accessibility

- All icons have `aria-label`.
- Color contrast meets WCAG AA for text and links.
- Focus outlines preserved for keyboard navigation.

## Implementation Notes

- Override Minima styles in `assets/main.scss`.
- Load Google Fonts via `<link>` in a custom head include.
- Update `_config.yml` Minima `social_links` if supported; otherwise hard-code icons in a custom header include.
- Update `index.md` intro copy to match prototype.
- No changes to post Markdown content required.

## Prototypes

- Home page: `.tmp/prototype-home.html`
- Article detail: `.tmp/prototype-post.html`

Both prototypes should be treated as throwaway visual references; final code lives in Jekyll includes and SCSS.
