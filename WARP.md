# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview
This is a minimalist portfolio website built with Astro 4.3. The portfolio is data-driven, pulling all content (personal info, experience, education, projects, skills) from a single JSON file (`cv.json`) that follows a structured schema.

## Commands

### Development
- `npm run dev` or `npm start` - Start dev server at `localhost:4321`
- `npm run build` - Type-check with Astro and build for production to `./dist/`
- `npm run preview` - Preview production build locally
- `npm run astro check` - Run Astro type checking

### Astro CLI
- `npm run astro ...` - Run Astro CLI commands (e.g., `npm run astro add`)

## Architecture

### Data-Driven Design
The entire portfolio is driven by `cv.json`, which contains all content in a standardized JSON Resume format. Components import and destructure data from this file using the `@cv` alias.

### Path Aliases
- `@cv` → `./cv.json` (portfolio data)
- `@/*` → `src/*` (source files)

### Component Structure
- **Pages**: `src/pages/index.astro` is the single-page portfolio
- **Layout**: `src/layouts/Layout.astro` provides the HTML shell with global styles
- **Sections**: Portfolio divided into modular components:
  - `Hero.astro` - Name, title, location, and social links
  - `About.astro` - Summary/bio
  - `Experience.astro` - Work history from cv.json
  - `Education.astro` - Educational background
  - `Projects.astro` - Project showcase with GitHub links
  - `Skills.astro` - Tech stack with icons
- **Utilities**: `sections/Section.astro` - Reusable section wrapper
- **Icons**: All social and tech icons are Astro components in `src/icons/`

### Icon System
Skills and social networks use a mapping pattern:
```typescript
const SKILLS_ICONS: Record<string, any> = {
  HTML, CSS, JavaScript, // etc.
}
```
Icon components must be imported and mapped by name to dynamically render based on cv.json data.

### Keyboard Shortcuts
The portfolio includes a command palette via `ninja-keys` library (Ctrl+K / Cmd+K):
- Navigate to social profiles with keyboard shortcuts (Ctrl+G for GitHub, Ctrl+L for LinkedIn, etc.)
- Print portfolio with Ctrl+P
- Mobile: floating button to trigger command palette

### Print Styling
The portfolio is print-optimized with `.print` and `.no-print` utility classes:
- `.no-print` - Hidden when printing (e.g., interactive elements)
- `.print` - Only visible when printing (e.g., contact info as text)

## Important Patterns

### Adding New Skills
1. Add skill to `cv.json` skills array with exact name matching
2. Create icon component in `src/icons/` if needed
3. Import and add to `SKILLS_ICONS` mapping in `Skills.astro`

### Adding Social Profiles
1. Add profile to `cv.json` basics.profiles array
2. Create icon component in `src/icons/` if needed
3. Add icon to both `Hero.astro` and `KeyboardManager.astro` SOCIAL_ICONS mappings

### Styling Convention
- Scoped `<style>` tags in each component
- Global styles in `Layout.astro`
- Mobile-first responsive design with `@media (width <= 700px)`
- Monospace fonts for body, sans-serif for headings
