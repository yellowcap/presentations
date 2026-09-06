# Welcome to [Slidev](https://github.com/slidevjs/slidev)!

To start the slide show:

- `pnpm install`
- `pnpm dev`
- visit <http://localhost:3030>

Edit the [slides.md](./slides.md) to see the changes.

Learn more about Slidev at the [documentation](https://sli.dev/).

## DevSeed Theme

### Theme Structure

```
theme/
├── components/
│   └── Logo.vue              # Reusable logo component
├── layouts/
│   ├── cover.vue             # Title slide with gradient background
│   ├── default.vue           # Standard content layout
│   └── text-image.vue        # Text left, image right layout
├── setup/
│   ├── main.ts               # App setup configuration
│   └── shiki.ts              # Code highlighting setup
├── styles/
│   └── index.css             # Brand colors and typography
├── custom-nav-controls.vue   # Laser pointer toolbar button
├── global-top.vue            # Laser pointer canvas
├── laser.ts                  # Laser pointer state and config
├── index.ts                  # Theme entry point
└── package.json              # Theme metadata
```

### Key Features

Brand Colors - All DevelopmentSeed colors are available:
- Primary: #CF3F02
- Secondary: #E2C044
- Success: #4DA167
- Info: #2E86AB
- Warning: #E2C044
- Danger: #A71D31

Typography:
- Roboto Condensed for headings
- Roboto for body text
- Roboto Mono for code

Layouts:
1. default - Standard content layout
2. cover - Title slides with gradient background
3. title - Title slides with image on bottom third
4. two-cols - Two-column layout with configurable gap and ratio

### Laser Pointer

Draws a red trail that fades out behind the cursor, for pointing at things while
presenting. Unlike Slidev's built-in drawing tool, the marks are temporary.

Two ways to use it:

- Click the squiggle button in the toolbar to toggle it on, then just move the pointer.
- Hold `Shift` and drag, without turning anything on.

In presenter mode the trail is mirrored to the audience window. It rides on
Slidev's presenter cursor sync, so "Show presenter cursor" must be enabled — the
toolbar button turns it on for you, `Shift`+drag alone does not.

Tune it from the headmatter of `slides.md`:

```md
---
theme: ./theme
themeConfig:
  laserColor: '#CF3F02'   # any CSS color, default '#ef4444'
  laserWidth: 8           # stroke width in px, default 6
  laserFade: 1500         # how long a mark lives in ms, default 900
  laserEnabled: true      # start with the laser on, default false
---
```

### Usage

To use the theme in any presentation:

```md
---
theme: ./theme
title: Your Presentation Title
---
```

For the text-image layout:

```md
---
layout: text-image
image: /path/to/image.jpg
---

# Your Content Here

```

Text on the left, image fills the right side.

To add your logo:

```
<Logo src="/logo.png" position="top-right" height="60px" />
```

I've also created slides-devseed-demo.md that demonstrates all the features. You can preview it by updating your slides.md or running the dev server to
see the theme in action!
