# Verification — Tarantula 8K Gamepad Viewer skin

## Outcome

The artwork, stylesheet references and native state compatibility checks passed. The deliverable contains a CSS stylesheet and nine SVG assets, with no skin HTML or JavaScript.

**Verification scope:** Gamepad Viewer's current, unmodified `tester.js` and `queue.js` were executed in an isolated Node VM using DOM stubs constructed from the current production controller template. The tests resolved the skin's class-based control rules against the resulting native classes. This checks the existing state updater's behavior and its compatibility with the skin. It is not an end-to-end browser or hardware test.

Live Chromium testing was unavailable in the execution environment. No physical Tarantula 8K, OBS instance, or publicly hosted copy of the finished skin was available for a live integration session. Hosted delivery and the physical controller's Home/Share exposure remain to be confirmed on the target computer.

## Passed native-state checks

| Check | Result |
| --- | --- |
| All 16 supported digital channels independently select pressed sprite rows | Passed |
| Both moving stick caps retain their click highlights and native margins/rotation | Passed |
| Releasing one button preserves every other held state and both stick offsets | Passed |
| Releasing all controls restores neutral artwork and centered stick offsets | Passed |
| D-pad diagonal lights the two correct quadrants | Passed |
| Left and right triggers retain different simultaneous analog values | Passed |
| Native trigger-meter clipping at 0%, 25%, 50%, 75% and 100% | Passed |
| Both triggers return to zero opacity when released in normal mode | Passed |
| Native deadzone and uncurved stick movement remain available | Passed |
| Active trigger layers are contained behind the bumper layer | Passed |
| Documented HTTPS stylesheet address passes the current URL loader; all nine relative asset paths resolve | Passed |

The input channel names, order and mapping were inspected in the current production [HTML](https://gamepadviewer.com/), [input mapper](https://gamepadviewer.com/gamepad.js), [state updater](https://gamepadviewer.com/tester.js) and [CSS loader](https://gamepadviewer.com/main.js).

## Visual and file checks

- Combined neutral and simultaneous-input artwork rendered and inspected at 1000 × 700.
- Overlay-size artwork rendered and inspected at 450 × 315.
- Transparent alpha confirmed both outside the controller and in the opening between the grips.
- All nine SVG files parsed as valid XML and use vector geometry; no embedded raster images, scripts, foreignObject elements or font-dependent text are required.
- Each CSS background asset exists at its referenced relative location.
- Normal and pressed artwork crops stay in their assigned sprite cells.
- Independent source/CSS review checked selector scope, native stick margins/rotation, trigger opacity/clipping, root scaling/visibility and Share's conditional native channel.
- A trigger/bumper layering issue found during review was fixed by adding `z-index: 0` to the trigger container.
- The preview PNGs are static artwork renders, not screenshots of a live physical-controller session.

## Source snapshot identifiers

The following SHA-256 values identify the upstream files used for these checks:

```text
tester.js
ad98b5f611dd69b6aca0bb07423f79f19908990a114062f6862a423ee48ce9aa

queue.js
181ee18358ecef2eb9beb50e5b116168c0662f5970c102898b78d4d085ff60fe

main.js
a289ec04cdfd4c651e91cb2c1b725035205c4a6af5522a49e2cf45eed992045d

Gamepad Viewer HTML
a2b9231d0438906efb093d7e74515bdb2902ce63fd1490bca0574b37fc904f8c
```

Upstream input code and test stubs are not part of the hosted skin.
