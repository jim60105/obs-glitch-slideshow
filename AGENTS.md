# OBS Glitch Slideshow - Agent Instructions

## Project Overview

This is a standalone HTML5 slideshow application designed for use in OBS Studio's browser source. The project provides a Codrops-inspired glitch transition effect for cycling through images with customizable timing.

**Key characteristics:**
- **Type**: Single-page web application (no build process)
- **Size**: Minimal - 4 files total
- **Language**: Vanilla HTML5, CSS3, JavaScript (ES6+)
- **Target Runtime**: Chromium-based browsers (OBS Studio Browser Source)
- **License**: GNU GPL v3

## Project Structure

```
obs-glitch-slideshow/
├── index.html       # Main slideshow application (single file, ~580 lines)
├── playlist.json    # Slideshow configuration (JSON)
├── LICENSE          # GNU GPL v3 license
└── README.md        # User documentation
```

**Important Files:**
- `index.html` - Contains all HTML, CSS, and JavaScript. Self-contained application.
- `playlist.json` - Data file defining slide images and display durations
- `README.md` - End-user documentation for OBS setup and customization

## Architecture

### HTML Structure
The application uses a layered approach:
1. **Base slideshow container** (`#slideshow`) - Contains slide `<div>` elements with `<img>` children
2. **Transition overlay** (`#transition-overlay`) - Contains 5-layer glitch effect structure
3. **Noise canvas** (`#noise-canvas`) - Dynamic noise generation during transitions

### CSS Architecture
- **CSS Variables** (`:root`) - Configurable glitch effect parameters
  - `--gap-horizontal`, `--gap-vertical` - Displacement values
  - `--time-anim` - Animation duration (must match `TRANSITION_DURATION` constant)
  - `--blend-mode-*`, `--blend-color-*` - Layer blend modes and colors
- **5-layer glitch system** - Based on Codrops CSSGlitchEffect demo3
  - Layer 1: Base image with gradual fade-in (must be 100% size)
  - Layers 2-4: Glitch effects with clip-path animations
  - Layer 5: Flash effect
- **Critical sizing rule**: `.glitch__img:nth-child(1)` must use `width: 100%; height: 100%; top: 0; left: 0` to match slide image size exactly

### JavaScript Architecture
- **No dependencies** - Pure vanilla JavaScript
- **Async/await** for playlist loading
- **State management** - Global variables: `currentIndex`, `playlist`, `slideElements`
- **Timing logic**:
  - Slide displays for `duration` seconds
  - Transition runs for `TRANSITION_DURATION` ms (3000ms)
  - Next transition scheduled after: `duration + TRANSITION_DURATION`
- **Canvas animation** - requestAnimationFrame for noise generation

## Code Style Conventions

### HTML
- Use semantic HTML5 elements
- `lang="zh-TW"` attribute (Traditional Chinese locale)
- Inline CSS and JavaScript (single-file architecture)

### CSS
- Use CSS custom properties for configuration
- BEM-like naming for glitch components (`.glitch__img`)
- Alphabetically sorted properties (enforced by formatter)
- Use `calc()` for dynamic sizing
- Keyframe animations named with descriptive suffixes: `-with-opacity`, `-reveal`

### JavaScript
- ES6+ syntax (const/let, arrow functions, async/await)
- UPPER_CASE for constants
- camelCase for functions and variables
- Async functions return Promises
- Comments in English
- Error handling with try/catch for async operations

## Configuration Points

### Timing Configuration
Two places must be kept in sync:
1. `TRANSITION_DURATION` constant (JavaScript, line ~372)
2. `--time-anim` CSS variable (CSS, line ~12)

### Playlist Format
```json
[
  {
    "image": "filename.ext",
    "duration": 5
  }
]
```
- `image`: Relative path to image file
- `duration`: Display time in seconds (before transition starts)

## Critical Implementation Details

### Image Sizing Consistency
**Problem**: The base glitch layer must match slide image size exactly to avoid visual jumps at transition end.

**Solution**: Layer 1 of glitch overlay uses:
```css
.glitch__img:nth-child(1) {
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
```
Other layers use offset sizing with gaps.

### Timing Sequence
The timing must follow this exact pattern:
1. Slide displays for `duration` seconds
2. Transition starts (old slide becomes `.previous`, overlay activates)
3. After `TRANSITION_DURATION`: new slide becomes `.active`, cleanup occurs
4. New slide displays for its `duration` seconds
5. Repeat

**Implementation**: `scheduleNextTransition()` uses nested `setTimeout`:
- Outer: waits for `duration`
- Inner: waits for `TRANSITION_DURATION` before scheduling next

### Glitch Effect Layers
1. **Base layer** - Fades in from 0% to 100% opacity
2. **Layers 2-4** - Clip-path polygon animations with opacity fade-out
3. **Layer 5** - Brief flash at start

All animations must:
- Start at `opacity: 0`
- End at `opacity: 0` (except layer 1, which ends at `opacity: 1`)
- Use `forwards` fill-mode

## Testing & Validation

### Manual Testing Procedure
1. Open `index.html` in Chrome/Edge
2. Place test images (`slide1.png`, `slide2.png`, `slide3.png`) in same directory
3. Verify timing sequence: Each slide shows for 5s → 3s transition → next slide 5s
4. Check for visual continuity: No size jumps at transition end
5. Verify loop: After last slide, returns to first

### Common Issues
- **Image size jump at transition end**: Check layer 1 sizing (must be 100% x 100%)
- **Incorrect timing**: Verify `TRANSITION_DURATION` matches `--time-anim`
- **Slides skip too fast**: Ensure nested setTimeout structure in `scheduleNextTransition()`
- **Images don't load**: Check relative paths in `playlist.json`

## Modification Guidelines

### Adding New Glitch Effects
1. Create `@keyframes` with unique name
2. Add to appropriate `.glitch.active .glitch__img:nth-child(N)` selector
3. Ensure opacity starts and ends at 0 (except base layer)
4. Match duration to `var(--time-anim)`

### Changing Transition Duration
1. Update `TRANSITION_DURATION` constant (JavaScript)
2. Update `--time-anim` variable (CSS)
3. Both must match exactly

### Adding Features
- Keep single-file architecture
- Maintain vanilla JavaScript (no dependencies)
- Test in OBS Studio Browser Source
- Document in README.md

## Trust These Instructions

The information in this file has been validated against the working codebase. When working with this project:
- Follow the architecture patterns described above
- Maintain the single-file structure
- Keep timing configuration synchronized
- Test in actual browser before assuming issues
- Only search codebase if instructions are incomplete or contradictory
