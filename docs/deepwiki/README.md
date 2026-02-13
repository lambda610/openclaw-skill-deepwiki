# Bento - Personal Portfolio Website

## Executive Summary

**Bento** is a JAMStack personal portfolio website built with **Astro**, **React**, and **TypeScript**. It features a modern "bento grid" layout—a popular design pattern popularized by Apple's widget-based interfaces—where content is organized in a responsive grid of tiles/cards. The application supports multiple tile types (social links, images, videos, music, location maps, text quotes) and includes theme switching (light/dark mode), OpenGraph metadata generation, and mobile-responsive design.

## One-Liner

A beautiful, responsive personal portfolio website with a customizable bento grid layout supporting social links, media tiles, and dynamic content cards.

## Key Features

### 🎨 Bento Grid Layout System
- **Responsive grid** (16-column desktop / 8-column mobile) based on Tailwind CSS
- **5 tile size variants**: `2x2`, `4x1`, `4x2`, `2x4`, `4x4`
- **Container query support** for adaptive layouts
- **Framer Motion animations** for smooth transitions

### 🔗 Multiple Tile Types
- **SocialTile**: Social media profile links (GitHub, Twitter, Instagram, Spotify, YouTube, Figma, Dribbble, LinkedIn)
- **LinkTile**: Generic custom links with icon support (iconify strings or image URLs)
- **ImageTile**: Full-cover image galleries with title overlays
- **VideoTile**: Video thumbnails with play button overlays
- **MusicTile**: Spotify-style horizontal music player with animated equalizer
- **LocationTile**: Google Maps embed with centered location label
- **TextTile**: Quote or text content with author attribution

### 🌓 Theme System
- **Light/Dark mode toggle** with persistence via localStorage
- **CSS custom properties** for theming
- **System preference detection** on initial load
- **Smooth color transitions**

### 🔧 Developer Experience
- **TOML-based configuration** for bento sections
- **TypeScript** with strict typing
- **Biome.js** for linting and formatting
- **Husky** for git hooks
- **Hot reload** in development mode

### 📱 Responsive Design
- **Mobile-first** approach
- **Breakpoint-aware** sizing
- **Adaptive typography** (scales with viewport)

### 🔍 OpenGraph Integration
- **Metadata extraction** from external URLs
- **Screenshot generation** for missing OG images
- **Preview cards** for social sharing

## Tech Stack

| Category | Technology |
|----------|------------|
| Framework | Astro 5.17 |
| UI Library | React 19.2 |
| Styling | Tailwind CSS + UnoCSS |
| Animations | Framer Motion |
| Icons | Iconify |
| Language | TypeScript 5.0 |
| Package Manager | Bun |
| Testing | Built-in Astro testing |

## Project Structure

```
bento/
├── src/
│   ├── components/
│   │   ├── bento/
│   │   │   ├── tiles/          # Tile components
│   │   │   ├── BentoCard.tsx   # Base card container
│   │   │   ├── BentoGrid.tsx   # Grid layout renderer
│   │   │   ├── PlatformIcon.tsx
│   │   │   └── ProfileSidebar.tsx
│   │   ├── ThemeToggle.tsx     # Dark/Light mode toggle
│   │   └── ui/                 # Shared UI components
│   ├── data/
│   │   ├── bento.gen.ts         # Generated configuration
│   │   ├── bento.toml          # User configuration
│   │   ├── metadata.toml       # OpenGraph metadata
│   │   └── site.json           # Site metadata
│   ├── layouts/
│   │   ├── BaseLayout.astro    # Base HTML wrapper
│   │   └── BentoLayout.astro   # Bento-specific layout
│   ├── pages/
│   │   └── index.astro         # Main entry page
│   ├── types/
│   │   └── bento.ts            # TypeScript definitions
│   └── utils/
│       └── opengraph.ts        # OG metadata utilities
├── public/                      # Static assets
├── astro.config.mjs            # Astro configuration
├── uno.config.ts              # UnoCSS configuration
└── package.json
```

## Quick Start

```bash
# Install dependencies
bun install

# Start development server
bun run dev

# Build for production
bun run build

# Preview production build
bun run preview
```

## Configuration

Bento uses **TOML** files for configuration:

- `bento.toml`: Main bento grid sections and items
- `metadata.toml`: OpenGraph metadata for link previews
- `site.json`: Site-wide metadata

## License

Apache-2.0
