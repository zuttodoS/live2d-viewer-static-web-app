# Live2D Viewer - Static Web App

A single-file Live2D model viewer and VTuber tool. Just open `index.html` in your browser — no server, no build tools, no installation required.

## Quick Start

1. Go to https://zuttodos.github.io/live2d-viewer-static-web-app/ or open `index.html` in **Chrome** or **Edge**
2. Click the **folder icon** (top-left sidebar)
3. Click **"Browse Folder..."** and select a folder containing your Live2D model
4. Your model appears on screen — start exploring!

Your loaded model is automatically cached in the browser, so it reloads when you refresh the page.

## Tutorial

### Loading a Model

Your Live2D model folder should contain a `.model3.json` file along with its associated assets (`.moc3`, textures, motions, expressions, etc.). Click "Browse Folder..." and select the entire model folder.

### Moving, Zooming & Rotating

| Input | Action |
|-------|--------|
| **Middle mouse drag** | Move model |
| **Alt + Left click drag** | Move model (alternate) |
| **Scroll wheel** | Zoom in/out |
| **Right mouse drag** | Rotate model |

You can also use the **Transform** controls in the Load Model panel:
- **Zoom slider** — adjusts scale (syncs with scroll wheel)
- **Rotate slider** — adjusts rotation (syncs with right-drag)
- **D-pad buttons** — click and hold to move in 8 directions; center button resets position

The **fullscreen button** (⛶) at the bottom of the sidebar toggles fullscreen mode.

### Playing Animations

1. Click the **play icon** (second button in sidebar) to open the Animation panel
2. Click any **motion button** to play it
3. Choose a **loop mode**:
   - **Idle** — returns to idle animation after the motion finishes
   - **Loop** — repeats the selected motion continuously
4. Use the **volume slider** to adjust motion sound volume
5. Use the **pause button** to freeze/resume the current motion

### Setting Expressions

1. Click the **smiley face icon** in the sidebar
2. Click any expression button to apply it
3. Click **"Clear Expression"** to remove it

### Auto Emotion Expression

Automatically switches the model's expression based on your detected facial emotion:

1. In the Expression panel, check **"Enable auto expression from emotion"**
2. For each emotion (Happy, Sad, Angry, Surprise, Fear, Disgust, Neutral), select a model expression from the dropdown
3. Set a **threshold** (0-1) — the emotion score must exceed this value to trigger
4. The highest-scoring emotion above its threshold wins and switches the expression

**Detected emotions** (derived from MediaPipe blend shapes):
- **Happy** — smile + cheek squint
- **Sad** — mouth frown + brow raise + lip press + mouth stretch
- **Angry** — brow down + eye squint + jaw forward
- **Surprise** — wide eyes + jaw open + brow raise
- **Fear** — wide eyes + brow raise + mouth stretch
- **Disgust** — nose sneer + upper lip raise
- **Neutral** — inverse of all other emotions

### Face Tracking (Webcam)

> May requires serving over HTTP (see [Webcam/Mic Features](#webcammic-features-require-http) below)

1. Click the **camera icon** in the sidebar to open the Tracking panel
2. Select your webcam from the dropdown
3. Check **"Enable Tracking"**
4. Click **"Calibrate"** while looking straight at the camera with a neutral expression, closed mouth and closed eyes
5. Your face movements now control the Live2D model in real-time

Default tracking is mirrored (like looking in a mirror) — turn left, model turns left.

**Available tracking data** (52+ fields):
- Head & body rotation (pitch, yaw, roll)
- Eye blink, squint, wide open
- Eye gaze direction (look up/down/left/right)
- Eyebrow raise and lower
- Mouth: smile, frown, pucker, funnel, stretch, press, roll, shrug, open
- Cheek squint
- Jaw open, forward, left, right
- Nose sneer
- 7 emotion scores (happy, sad, angry, surprise, fear, disgust, neutral)

All fields can be mapped to any Live2D parameter in the **Parameter Mapping** panel.

### Hand Tracking

> Requires face tracking to be active (shares the same webcam feed)

1. Click the **hand icon** (✋) in the sidebar to open the Hand Track panel
2. Enable face tracking first (camera icon in sidebar)
3. Check **"Enable Hand Tracking"**

**Output modes:**
- **Draw Hands** (default) — draws your hand skeleton overlaid on the model in real-time
- **Map to Params** — exposes hand data as fields in the Parameter Mapping panel

**Draw Hands options:**
- **Mirror** — flips hands horizontally (default on)
- **3D Rendering** — depth-aware rendering with tapered bones, joint spheres, highlights, and shadows
- **Line Color / Thickness / Opacity** — customize the look
- **Size Scale** — scale the hand drawing relative to the model
- **D-pad + reset** — offset hand position on screen; hold button to keep moving

**Mapped hand fields** (available in Parameter Mapping when "Map to Params" is selected):

| Field | Description |
|-------|-------------|
| `handLeftX`, `handRightX` | Wrist X position (−1 = left, 1 = right) |
| `handLeftY`, `handRightY` | Wrist Y position (−1 = top, 1 = bottom) |
| `handLeftZ`, `handRightZ` | Wrist depth in meters ×5 (world space) |
| `handLeftRotationX`, `handRightRotationX` | Pitch — forward/back tilt (degrees) |
| `handLeftRotationY`, `handRightRotationY` | Yaw — left/right tilt (degrees) |
| `handLeftRotationZ`, `handRightRotationZ` | Roll — wrist rotation (degrees; 0° = fingers up) |
| `handLeftPalmFacing`, `handRightPalmFacing` | Palm facing camera: −1 = back of hand, 0 = side, 1 = palm |
| `handLeftThumbCurl` … `handRightPinkyCurl` | Per-finger curl: 0 = straight, 1 = fully curled |
| `handLeftGrab`, `handRightGrab` | Grab strength: average of 4 finger curls (0 = open, 1 = fist) |
| `handLeftDistance`, `handRightDistance` | Distance from camera: 0 = far, 1 = close |

You can bind a hotkey to **Toggle Hand Track** in the Hotkeys panel.

### Lip Sync with Vowel Detection

> May requires serving over HTTP (see [Webcam/Mic Features](#webcammic-features-require-http) below)

1. In the Tracking panel, select your microphone
2. Check **"Enable Lip Sync"**
3. Adjust **Gain** (sensitivity) and **Threshold** (minimum volume to trigger)
4. The detected vowel (A, I, U, E, O) lights up below the level bar

**Vowel Calibration** (optional, improves accuracy):
1. Hold a sustained "aaa" sound and click the **A** button
2. Repeat for I, U, E, O
3. Calibrated vowels show a checkmark
4. Click **Reset** to clear calibration

### Parameter Mapping

1. Click the **link icon** in the sidebar
2. **Model Parameters** section shows all Live2D parameters with interactive sliders
   - Drag a slider to manually override a parameter value
   - Double-click a slider to clear that override
   - Click **"Clear Overrides"** to reset all
3. **Mappings** section lets you connect tracking data to Live2D parameters
   - Left dropdown: tracking source (e.g., `mouthSmileLeft`, `jawOpen`, `emotionHappy`)
   - Right dropdown: Live2D parameter target
   - Output min/max auto-populate from the selected parameter's range when you change the target
   - Click **"+ Add"** to add a new mapping
   - Click **"Reset Defaults"** to restore default mappings

### dTrack (Motion Detection)

> May requires serving over HTTP (see [Webcam/Mic Features](#webcammic-features-require-http) below)

dTrack uses webcam optical flow to detect repetitive motion like hand waving and map it to a Live2D parameter:

1. Click the **film icon** in the sidebar
2. Select webcam and check **"Enable dTrack"**
3. Configure axis (X/Y), reverse, limiter, deadzone, smoothing
4. Select a **target parameter** to control

### Texture Replacement

1. Click the **picture icon** in the sidebar
2. See all textures currently used by the model
3. Click **"Replace Texture"** to swap with a new image
4. Click **"Reset"** to restore the original

### Drawing

1. Click the **pencil icon** (✏) in the sidebar
2. Check **"Enable Drawing"** to activate the drawing canvas
3. Select a tool: **Brush**, **Eraser**, **Line**, **Fill**, or **Text**

**Brush/Eraser features:**
- **7 presets** — Hard Round, Soft Round, Airbrush, Flat Brush, Pencil, Hard Eraser, Soft Eraser
- **Preset customization** — modify any preset's settings, click "Save Preset" to persist; customized presets show `*`; right-click a preset to reset it
- **Custom brush** — saves current settings as a new "Custom" preset in the list
- Adjustable size, opacity, hardness, flow, spacing
- **Brush tip** — ratio (aspect) and angle for flat/calligraphic brushes
- **Stylus pen support** — pressure controls size and/or opacity; tilt controls brush angle
- **Input Smoothing** slider — reduces pressure/tilt jitter for smoother strokes (0 = raw, 1 = very smooth, default 1)
- **Undo/Redo** (Ctrl+Z / Ctrl+Shift+Z)

**Other tools:**
- **Line** — drag to draw straight lines with adjustable width and opacity
- **Fill** — flood fill with configurable tolerance
- **Text** — click to place text with font, size, bold, italic, underline options

**Multi-layer support:**
- Add, delete, reorder layers (like Krita/Photoshop)
- Per-layer opacity slider and visibility toggle
- Drawing always targets the selected (active) layer
- **Clear Layer** clears only the active layer
- **Save Image** exports only visible layers as PNG

**Canvas controls:**
- **Color picker** with hex input and eyedropper (samples from screen)
- **Canvas Opacity** slider — fade the entire drawing canvas
- **Clear Layer** / **Save Image** (exports visible layers as PNG)
- **Reset Settings** — restores all drawing settings to defaults

The drawing canvas sits **behind the Live2D model** but in front of the background.

### Background

1. Click the **palette icon** in the sidebar
2. Adjust R/G/B sliders or enter a hex color
3. Quick options: **Green** (green screen), **Transparent** (alpha background)
4. Browse for a **background image**

### Hotkeys

1. Click the **keyboard icon** in the sidebar
2. Click **"Set"** next to any action
3. Press a key or key combination (e.g., Ctrl+1, Shift+A) to bind it

### Settings Export/Import

In the Load Model panel:
- **Export Config** — downloads all settings as a JSON file
- **Import Config** — loads settings from a previously exported JSON file

Settings include hotkey bindings, parameter mappings, emotion-expression mappings, background config, device selections, and vowel calibration data.

### Panel Resize

Drag the right edge of any open panel to resize its width (200px–800px).

## Webcam/Mic Features Require HTTP

Features that access your camera or microphone may need the page served over HTTP due to browser security. For a quick local server:

```bash
# Python
python -m http.server 5002

# or Node.js
npx serve -p 5002
```

Then open `http://localhost:5002` instead of the file directly.

### What works on file://
- Model loading, animations, expressions, sound playback
- Auto emotion expression (when tracking data available)
- Background, hotkeys, parameter mapping, texture replacement
- Settings export/import, model auto-reload from cache

### What may requires HTTP (when browsing from a strict device like Android phone)
- Face Tracking (webcam)
- Hand Tracking (webcam, requires face tracking active)
- Lip Sync (microphone)
- dTrack (webcam)

## Requirements

- **Chrome or Edge** (uses `webkitdirectory` for folder selection)
- Firefox may work but folder picker support varies

## Technical Details

Everything is inlined into a single `index.html` file (~704KB):
- Live2D Cubism SDK core runtime (~223KB)
- Bundled application JavaScript (~451KB, includes CSS)
- Model files cached in IndexedDB for persistence across refreshes

No external dependencies loaded from disk. MediaPipe WASM model is downloaded from CDN when face tracking is first enabled.

## About

**Author:** Ram Tran
**Email:** noobv2ram@gmail.com
**Discord:** http://discord.com/users/noobv2

## License

Live2D Cubism SDK is subject to the [Live2D Open Software License](https://www.live2d.com/eula/live2d-open-software-license-agreement_en.html).
