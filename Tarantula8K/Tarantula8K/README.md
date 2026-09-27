# Tarantula 8K — orange Gamepad Viewer skin

A flat, transparent vector overlay based on the supplied orange GameSir Tarantula 8K reference. The supplied Xbox overlay informed the amount of visual detail. The controller silhouette, control positions and markings follow the Tarantula reference.

The production skin consists of **style.css and nine SVG files**. It runs inside Gamepad Viewer. There is no separate HTML application, JavaScript, input polling, or replacement controller logic.

## Files

```text
Tarantula8K/
  style.css
  README.md
  Verification.md
  Preview.png
  PreviewActive.png
  assets/
    Shell.svg
    FaceButtons.svg
    Dpad.svg
    Sticks.svg
    CenterButtons.svg
    Home.svg
    Bumpers.svg
    Triggers.svg
    TriggersActive.svg
```

Only `style.css` and `assets/` are required at runtime. The PNGs are static artwork previews; the active preview illustrates simultaneous inputs.

## Hosting and exact URL structure

Keep `style.css` next to the `assets` folder. SVG paths are relative to the CSS file, so the package can be hosted at any HTTPS static directory without editing the stylesheet.

For a GitHub Pages repository named `gamepad-skins`, with the complete `Tarantula8K` folder at the repository root:

```text
https://YOUR-USERNAME.github.io/gamepad-skins/Tarantula8K/style.css
```

The corresponding Gamepad Viewer URL is:

```text
https://gamepadviewer.com/?p=1&sc=1&css=https%3A%2F%2FYOUR-USERNAME.github.io%2Fgamepad-skins%2FTarantula8K%2Fstyle.css
```

Replace `YOUR-USERNAME` and, if necessary, `gamepad-skins` with the actual host account and repository name. These are URL templates, not already published locations.

General format:

```text
https://gamepadviewer.com/?p=1&sc=1&css=URL_ENCODED_HTTPS_ADDRESS_OF_STYLE_CSS
```

An alternative that avoids manually encoding the CSS address is Gamepad Viewer's **Generate URL** panel: set **Player Number** to the connected controller, **Scale Modifier** to `1`, and **Custom CSS URL** to the ordinary HTTPS stylesheet address. Use the generated result.

Use an actual CSS file served by an HTTPS static host. A GitHub `blob` page is a code-viewing webpage. Current Gamepad Viewer also rewrites raw GitHub stylesheet addresses to the obsolete `rawgit.com` service, so GitHub Pages is the appropriate GitHub hosting URL. This behavior is visible in [Gamepad Viewer's loader](https://gamepadviewer.com/main.js).

## OBS setup

1. In OBS, add a **Browser** source and leave **Local file** off.
2. Set **URL** to the complete Gamepad Viewer URL above.
3. Set **Width** to **1000** and **Height** to **700**. A custom frame rate of **60 FPS** is suitable for a 60 FPS stream.
4. Keep OBS's default Browser Source Custom CSS. This skin loads through the URL; its stylesheet does not need to be pasted into OBS.
5. Position and resize the source in the OBS scene. The background is transparent, so no chroma-key filter is needed.
6. Press a controller button to let Gamepad Viewer detect it. If it is not the first controller, use `p=2`, `p=3`, or `p=4`, or select the correct player with Gamepad Viewer's URL generator.

For a smaller source viewport, **500 × 350** with `sc=0.5` preserves the same proportions and native stick travel. For normal scene editing, keeping the source at 1000 × 700 and scaling it in the scene is convenient.

If detection needs focus, OBS's **Interact** action opens the Browser Source's interaction view. Hardware/driver button exposure and controller index must be confirmed on the computer running OBS.

OBS documents the URL, viewport size, frame rate and transparency fields in its [Browser Source guide](https://obsproject.com/kb/browser-source).

## What responds to input

| Tarantula control | Existing Gamepad Viewer element | Native input channel |
| --- | --- | --- |
| A / B / X / Y | `.button.a` / `.b` / `.x` / `.y` | Buttons 0 / 1 / 2 / 3 |
| Left / right bumper | `.bumper.left` / `.right` | Buttons 4 / 5 |
| Left / right trigger | `.trigger.left` / `.right` | Buttons 6 / 7, analog values |
| View | `.back` | Button 8 |
| Menu | `.start` | Button 9 |
| Left / right stick click | `.stick.left` / `.right` | Buttons 10 / 11 |
| D-pad up / down / left / right | `.face.up` / `.down` / `.left` / `.right` | Buttons 12 / 13 / 14 / 15 |
| Home / Guide | `.meta` | Button 16, when exposed |
| Share | `.touchpad` | Existing button 17 channel, when exposed or remapped |
| Left stick motion | `.stick.left` | Axes 0 and 1 |
| Right stick motion | `.stick.right` | Axes 2 and 3 |

These are Gamepad Viewer's default logical channels. Its [native input mapping](https://gamepadviewer.com/gamepad.js) determines how reported values reach each element; a controller mode or user remapping can change the physical source.

Pressed controls switch from dark/orange faces to pale warm highlights. Each control has an independent state. D-pad diagonals illuminate both relevant directions. Stick-click highlights stay attached to the moving stick cap.

Triggers use Gamepad Viewer's native analog opacity. For bottom-to-top trigger fill instead, append `&smeter=1` to the Gamepad Viewer URL. Resting trigger artwork remains visible behind the live fill in either mode.

## Share, macro controls and touch-area marking

The three small center controls are depicted as **View, Share and Menu**. The upper black panel and four slanted orange buttons are macro controls. They are not represented as a touchpad surface. This layout agrees with the [manufacturer manual transcription](https://manuals.plus/gamesir/t3-ce-tarantula-8k-pc-wired-esports-controller-manual) and the supplied reference image.

The physical Share button uses Gamepad Viewer's existing extra button slot, whose internal selector happens to be `.touchpad`. It highlights only if the controller/driver reports that channel or an available input is assigned to it with Gamepad Viewer's native **Button Rebinding** tool. Home/Guide likewise depends on the browser receiving its input. Availability from the user's physical Tarantula has not been measured in this environment.

The top macro controls are included in the shell artwork. When a programmable control emits A, a bumper, a stick click, or another standard action, the corresponding standard control highlights. Gamepad Viewer does not provide independent display nodes for every physical programmable control. Their separate physical location cannot be inferred from an input shared with another button.

The **TOUCH AREA** marking above the right stick is retained visually. This skin does not invent a capacitive-contact event. Any controller-side assignment that emits an ordinary supported input will be displayed through that input's normal element.

## Implementation notes

- The custom stylesheet targets `.controller.custom` and Gamepad Viewer's existing descendants.
- Neutral stick positions use `left` and `top`. Native inline movement margins and rotation transforms remain intact.
- `.pressed` changes the sprite row, independently for each button and stick.
- Trigger resting artwork uses container pseudo-elements. Native trigger spans retain control of opacity and clipping. The trigger container has its own stacking context so its highlight cannot cover the bumpers.
- `.disconnected` applies a faded appearance. Connection detection and element creation remain native.
- All SVG artwork is vector geometry. No raster artwork, external fonts, animation libraries or third-party runtime dependencies are required by the skin.

## Technical references

- [Gamepad Viewer and URL generator](https://gamepadviewer.com/)
- [Current custom-CSS loader](https://gamepadviewer.com/main.js)
- [Current native state updater](https://gamepadviewer.com/tester.js)
- [Current input mapping](https://gamepadviewer.com/gamepad.js)
- [Requested DualSense implementation reference](https://github.com/brendanwelsh/playstation-ds5-white/blob/main/style.css)
- [Independent Pro Controller skin](https://github.com/shmove/gamepadviewer-procon/blob/master/procon.css)

The GitHub skins were inspected for implementation conventions. Their controller artwork and visual designs were not copied.
