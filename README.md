# OBS Glitch Slideshow

A local HTML slideshow designed for use in OBS Studio browser source, featuring a stunning glitch transition effect inspired by [Codrops CSSGlitchEffect](https://github.com/codrops/CSSGlitchEffect).

## Features

- **Local HTML file** - No server required, works directly from your file system
- **Customizable playlist** - Control slide order and duration via JSON file
- **Codrops-style glitch transition** - 3-second transition with layered glitch animations
- **Gradual image replacement** - New image progressively fades in while glitch effects play
- **Transparent background** - Perfect for overlay use in OBS
- **Responsive design** - Adapts to any resolution

## Glitch Effect Details

The transition uses a 5-layer glitch effect based on Codrops CSSGlitchEffect demo3:

1. **Base Layer**: The new image gradually fades in from 0% to 100% opacity over 3 seconds
2. **Glitch Layer 2**: Horizontal slice animation with rightward displacement
3. **Glitch Layer 3**: Horizontal slice animation with leftward displacement
4. **Glitch Layer 4**: Inverted/mirrored slice animation with vertical displacement
5. **Glitch Layer 5**: Brief flash effect at the start of the transition

The effect combines:

- Animated noise canvas overlay
- Multiple clip-path polygon slice animations
- Gradual opacity transitions for smooth image replacement
- Color blend modes for visual interest

## File Structure

```text
obs-glitch-slideshow/
├── index.html       # Main slideshow HTML file
├── playlist.json    # Slideshow configuration
├── slide1.jpg       # Your image files
├── slide2.jpg
├── slide3.jpg
└── ...
```

## Playlist Configuration

Create a `playlist.json` file in the same directory as `index.html`:

```json
[
  {
    "image": "slide1.jpg",
    "duration": 5
  },
  {
    "image": "slide2.jpg",
    "duration": 8
  },
  {
    "image": "slide3.jpg",
    "duration": 5
  }
]
```

### Playlist Options

| Property   | Type   | Description                                            |
| ---------- | ------ | ------------------------------------------------------ |
| `image`    | string | Image filename (relative to index.html)                |
| `duration` | number | Display duration in seconds (before transition starts) |

## OBS Setup

1. Copy all files to a local folder
2. Add your images to the folder
3. Configure `playlist.json` with your images and durations
4. In OBS Studio:
   - Add a **Browser** source
   - Check **Local file**
   - Browse to select `index.html`
   - Set width/height to match your canvas (e.g., 1920x1080)
   - Uncheck **Shutdown source when not visible** (optional)
   - Uncheck **Refresh browser when scene becomes active** (optional)

### Recommended OBS Browser Source Settings

| Setting    | Value                        |
| ---------- | ---------------------------- |
| Width      | 1920 (or your canvas width)  |
| Height     | 1080 (or your canvas height) |
| FPS        | 60                           |
| Custom CSS | (leave empty)                |

## Supported Image Formats

- JPEG (.jpg, .jpeg)
- PNG (.png)
- GIF (.gif)
- WebP (.webp)

## Customization

### Transition Duration

Edit the `TRANSITION_DURATION` constant in `index.html`:

```javascript
const TRANSITION_DURATION = 3000; // 3 seconds (in milliseconds)
```

### Glitch Effect CSS Variables

The glitch effect can be customized via CSS variables in the `:root` selector:

```css
:root {
  --gap-horizontal: 10px;    /* Horizontal displacement of glitch slices */
  --gap-vertical: 5px;       /* Vertical displacement of glitch slices */
  --time-anim: 3s;           /* Animation duration */
  --blend-mode-5: overlay;   /* Blend mode for flash layer */
  --blend-color-5: #af4949;  /* Color for flash layer */
}
```

### Playlist Filename

Edit the `PLAYLIST_FILE` constant in `index.html`:

```javascript
const PLAYLIST_FILE = 'playlist.json';
```

## Browser Compatibility

Designed for Chromium-based browsers (used by OBS Browser source). Tested with:

- OBS Studio 28.0+
- Google Chrome 90+
- Microsoft Edge 90+

## License

This project is open source and available under the [MIT License](LICENSE).
