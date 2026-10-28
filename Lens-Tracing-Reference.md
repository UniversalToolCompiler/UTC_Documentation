What the [tutorial](Lens-Tracing-Tutorial) leaves out: every camera setting, the console variables, the limits, and what is coming next.

## More troubleshooting

The tutorial's *Quick answers* cover the common cases. The rarer ones:

| Problem | Fix |
| --- | --- |
| A black band over part of the image | Unreal's own motion blur is on (`r.LensTracer.MotionBlur 2`). Console: `r.LensTracer.MotionBlur 0` (the default), or 1 for the lens tracer's. |
| No motion blur | It is off by default for now: console `r.LensTracer.MotionBlur 1`. Then Play, or press **G** in the viewport (Game View); the viewport must be Realtime and the game not paused, and **Amount** and **Max** in the camera's **Post Process > Rendering Features > Motion Blur** must be above 0. |
| No colour fringes | They are off by default for now: console `r.LensTracer.ChromaticAberration 1`. The lens needs Abbe Numbers too. **Post Process > Lens > Chromatic Aberration** is Unreal's screen effect, not the lens's. |
| A moving object keeps hard edges and a faint trail | A known limit of the motion blur: it can't see what is behind a moving object. Camera moves blur correctly. |
| Edges of bright objects flicker | Console: `r.LensTracer.TemporalDepth 1`. |
| Smeared columns at the sides of a Movie Render Queue frame, or frames larger than the output | Render at the sensor's aspect ratio; with High Resolution tiles, set **Camera > Overscan Percentage Override** to 10 %. See the tutorial's *Shooting a sequence*. |

Still stuck? Open **Window > Output Log** and search for `LensTracer`. Its warnings say what is wrong with a lens.

## Need help?

Contact: [contact@universaltoolcompiler.org](mailto:contact@universaltoolcompiler.org).

Please send:

- Your Unreal version and graphics card.
- A screenshot of the problem.
- The lines containing `LensTracer` from the Output Log.
- Which lens you used, and any console commands you changed.

## Every camera setting

The **Lens Tracing** section, from top to bottom: six dropdowns, each with its everyday settings first. **Focus** keeps its debug display in a **Debug** dropdown and **Exposure** its rarer settings in an **Advanced** one (closed until you open them; the editor remembers); **Sensor** holds a **Filmback** dropdown (Recommended Sensor, then the sensor size) and a **Noise** dropdown of its own, with its own Advanced. The rows marked *Cine Camera* or *Post Process* are those components' own settings, moved into the section: Sequencer and Blueprints see the same properties. The motion blur's settings and the Chromatic Aberration switch are hidden for now (see the tutorial's *Shooting a sequence* and *Tame the lens*).

| Dropdown | Setting | What it does |
| --- | --- | --- |
| Lens | Lens Data | The lenses this camera can mount. A new camera has **Standard_50mm**. |
| | Active Lens | Index of the mounted lens. Keyframeable. Out of range: an ordinary, untraced Cine Camera. |
| Iris | Bladed Iris | Ticked (default): polygonal bokeh as the iris closes. Off: a round iris at every f-number; Blade Rotation greys out. |
| | Diaphragm Blade Count (*Cine Camera*) | Iris blades, 4 to 16. Above the lens's widest f-number the bokeh becomes a rounded polygon, straighter as the aperture closes; its area keeps the f-number's brightness. Used with Focus Method Manual (the default) or Tracking; with the other methods the post process settings give the blades and the round-iris f-number (**Number of diaphragm blades**, **Maximum Aperture (min F-stop)**). |
| | Blade Rotation | Turns the iris, in degrees; positive values turn the bokeh counterclockwise. 0: a flat blade edge at the bottom of background bokeh. Keyframeable. |
| Focus (*Cine Camera*) | Focus Method, Manual Focus Distance (eyedropper) | The plane in focus. With no focus distance, the sensor stays at the lens's last Thickness (infinity for the included lenses). For Focus Method **Tracking**, the tracked actor is in **Current Camera Settings > Focus Settings**. |
| | Zoom Focal Length | Zoom lenses only (the row shows while one is mounted): the focal length the zoom is set to, as marked on the lens, kept within its range; the box's slider spans that range. Keyframeable. Default 35 mm. |
| | *Debug:* Draw Debug Focus Plane, Debug Focus Plane Color | Shows the plane in focus in the viewport. |
| Exposure | Metering Mode (*Post Process*) | Unreal's exposure method. Manual makes ISO, aperture and shutter speed set the brightness. Tick to override. |
| | Auto ISO | Ticked (default): with an automatic Metering Mode, the camera picks the ISO the brightness needs and shows it next to the box while you look through it; the sensor noise uses that ISO. Off, or with Manual metering: the ISO below. |
| | ISO | Unreal's exposure (Manual metering) and the sensor's read noise. Greyed while Auto ISO picks the ISO. Keyframeable. Default 100. |
| | Current Aperture (*Cine Camera*) | The f-number. Clamped to the lens's widest f-number (a zoom: its widest anywhere in the range; at each zoom setting the lens opens at most as wide as it can there). Keyframeable. |
| | Shutter Speed (1/s) | Exposure time 1/Shutter Speed: Unreal's exposure, the sensor noise and the lens tracer's motion blur (off by default for now). Keyframeable. Default 60. |
| | *Advanced:* Exposure Method (*Cine Camera*) | Enabled (default): the camera writes its aperture, shutter speed and ISO into the view. Do Not Override: Post Process Volumes keep them. |
| | *Advanced:* Exposure Compensation (*Post Process*) | Exposure offset in stops. 0 and ticked on a new lens tracing camera: a neutral exposure, where Unreal's default adds one stop. |
| | *Advanced:* Min EV100, Max EV100 (*Post Process*) | The darkest and the brightest scene the automatic exposure follows, in EV100. Past them the image gets darker or brighter, and Auto ISO stops at the matching ISO. Tick to override. Defaults -10 and 20. (Min Brightness and Max Brightness, in cd/m², in a project without the extended luminance range.) |
| | *Advanced:* Speed Up, Speed Down (*Post Process*) | How fast the automatic exposure follows a scene that gets brighter or darker, in stops per second. Tick to override. Defaults 3 and 1. |
| Sensor | Filmback (*Cine Camera*) | The sensor size. A sensor larger than the image circle shows its dark edge. |
| | *Filmback:* Recommended Sensor | Ticked: the filmback follows the mounted lens, the largest preset its image circle covers (a zoom: over its whole range); the preset and the sensor size grey out. Off by default. |
| | *Noise:* Sensor Noise | Ticked: the sensor's noise. Off by default. |
| | *Noise:* Base ISO | The sensor's native sensitivity: the ISO at which its photosites fill up. Greyed while Sensor Noise is off. |
| | *Noise > Advanced:* Pixel Pitch, Full Well Capacity, Read Noise, Read Noise High ISO, Dark Current, Gain Variation, Color Filter Array, Daylight White Balance | The rest of the sensor; the defaults are a full-frame CMOS sensor of about 24 megapixels. Greyed while Sensor Noise is off. |
| Lens Effects | Focus Breathing | On (default): the field of view follows focus, as on a lens that moves as a whole to focus. Off: fixed at the effective focal length. |
| | Distortion | The lens's real distortion; the camera renders the extra margin it needs and crops it. |
| | Mechanical Vignetting | Lens element edges cut the light off axis: cat's eyes and dark corners. |
| | Natural Vignetting | Light reaches the corners of the sensor at a slant: the gentle cos⁴ falloff every lens has, from its real chief rays. Off: even illumination. |
| | Field Curvature | Off-axis focus on the lens's curved field: the Petzval swirl. Needs Distortion. |

Also set from the lens, in **Current Camera Settings > Lens Settings**: Min FStop (its widest f-number, where the iris is round), the focal length range (one focal length; a zoom's range) and Squeeze Factor (1). **Current Camera Settings** also keeps **Current Focal Length**, locked because the lens sets it (with Focus Breathing, the framing focal length at the current focus), and the Cine Camera's **Focus Settings**: the tracked actor for Focus Method **Tracking**, Smooth Focus Changes, Focus Smoothing Interp Speed and Focus Offset.

The plugin uses the view target's first active camera component, as the engine does. Cameras saved before the camera had its own Shutter Speed and ISO keep their Post Process values: they move to the camera when the level loads.

## Console variables

Type them in the console, or set them in `DefaultEngine.ini` under `[SystemSettings]`. Five lens effects (Distortion, FieldCurvature, Rims, Blades, NaturalVignetting) also have a switch on each camera: at 0 the variable turns the effect off for every camera, at 1 each camera decides. ChromaticAberration has no camera switch for now: the variable alone turns it on or off.

| Variable | Default | What it controls |
| --- | --- | --- |
| `r.LensTracer` | 1 | The whole plugin. 0: engine depth of field as usual. |
| `r.LensTracer.SampleDensity` | 1.0 | Rays per pixel of blur area, capped by the lens's Max Rays Per Pixel. |
| `r.LensTracer.Pyramid` | 1 | Large blurs gathered at lower resolution: faster and cleaner. 0: full resolution only. |
| `r.LensTracer.TemporalDepth` | 1 | Removes TSR/TAA jitter from silhouette depth. 0: bright edges may flicker. |
| `r.LensTracer.Distortion` | 1 | Lens distortion, with automatic overscan and crop, where the camera's Distortion is ticked. 0: undistorted image everywhere. |
| `r.LensTracer.FieldCurvature` | 1 | Off-axis focus follows the curved field and astigmatism, where the camera's Field Curvature is ticked. 0: one flat focus plane everywhere. Needs the distortion. |
| `r.LensTracer.Rims` | 1 | Element edges cut the light off axis (cat's eyes and vignetting), where the camera's Mechanical Vignetting is ticked. 0: only the iris limits rays, everywhere. |
| `r.LensTracer.Blades` | 1 | Iris blades from Diaphragm Blade Count, round at the lens's widest f-number, where the camera's Bladed Iris is ticked. 0: always round. |
| `r.LensTracer.NaturalVignetting` | 1 | The cos⁴ falloff toward the corners, where the camera's Natural Vignetting is ticked. 0: even illumination everywhere. |
| `r.LensTracer.ChromaticAberration` | 0 | 0: none, the default for now (chromatic aberration is being optimized; its camera switch is hidden): every colour through the same lens. 1: colour fringes from the lens's Abbe numbers, purple and green on blurred edges and highlights, colour edges toward the corners, on every camera and in the Lens Editor's preview; focus is set for green, and the lens renders about 2.5 times slower. |
| `r.LensTracer.MotionBlur` | 0 | 0: none, the default for now (motion blur is being optimized; its camera settings are hidden). 1: the lens tracer's own motion blur, from Shutter Speed, with the camera's rolling shutter. 2: Unreal's motion blur instead (part of the image can turn black). |
| `r.LensTracer.SensorNoise` | 1 | 1: sensor noise on the cameras whose Sensor Noise is ticked. 0: never. 2: also on views traced through the default lens (`r.LensTracer.DefaultLens`). |
| `r.LensTracer.AutoISO` | 1 | 1: Auto ISO where the camera's Auto ISO is ticked. 0: the noise always uses the ISO set. |
| `r.LensTracer.DefaultLens` | 0 | 1: trace every perspective view without a lens tracing camera through the 24-48mm zoom, at 24 mm. A debug aid; a packaged game traces only if that lens is cooked with it. |
| `r.LensTracer.Debug` | 0 | Debug views, below. |

**Debug views** (`r.LensTracer.Debug`):

| Value | What it shows |
| --- | --- |
| 1 | Red: blur radius (1.0 = 10 pixels). Green: rays used / Max Rays Per Pixel. Blue: resolution level of large blurs. |
| 2 | Sensor. Green: distance from centre / image-circle radius. Blue: glass edges cut the aperture. Red: outside the image circle. |
| 3 | Share of the aperture the glass edges let through. White: all of it. |
| 4 | Pixels split across a silhouette. Red: near side. Green: far side. Black: one depth. |
| 5 | Sensor noise: signal-to-noise ratio per channel, brighter is cleaner (white: 255 and more). The lens renders normally. Use Manual metering. |
| 6 | Shutter, with `r.LensTracer.MotionBlur 1`. Red: motion during the exposure (1.0 = 20 pixels). Green: rays used / Max Rays Per Pixel. Blue: rolling-shutter shift (1.0 = 20 pixels). |
| 8 | Chromatic aberration, with `r.LensTracer.ChromaticAberration 1`. Red: how far apart the red and blue images are (1.0 = 5 pixels). Green: rays used / Max Rays Per Pixel. Blue: how much the blur differs between the colours (1.0 = 5 pixels). |

Views 1 to 3 are also in the Lens Editor's preview (Blur radius, Image circle, Vignetting), without the console.

## See-through objects

The lens reads the opaque depth buffer, so a window at 1.5 m in front of a wall at 30 m is blurred as if it were at 30 m. Two workarounds:

1. **Material writes depth.** Enable **Output Depth and Velocity** on the translucent material. Catch: it can no longer read Scene Depth (Depth Fade and Scene Depth refraction setups fail to compile), and what is seen through it takes its depth.
2. **Depth proxy.** Duplicate the mesh with an invisible material: Additive, Unlit, Emissive 0, Opacity 1, Output Depth and Velocity on. The real material stays unchanged.

## Performance

Cost grows with output resolution and rays per pixel; sharp areas are cheap. RTX 3090, 64 rays per pixel:

| Output | In focus | Blurred |
| --- | --- | --- |
| 1920×1080 | about 0.7 ms | about 1.8 ms |
| 3840×2160 | — | about 7.2 ms |

For previews lower `r.LensTracer.SampleDensity` or the lens's Max Rays Per Pixel (64 or 128); keep `r.LensTracer.Pyramid 1`.

Chromatic aberration, when turned on, makes the lens about 2.5 times as costly (it follows each colour on its own), which is why it is off by default for now. Iris blades add about 0.35 ms at 1920×1080 (f/4). Motion blur, when turned on: a moving camera costs about 1.3 ms at 1920×1080 whatever its speed, more where objects close to the camera cross the view; moving objects cost about one ray per pixel of their streaks. Sensor noise costs about 0.05 ms at 1920×1080.

## Requirements

| Requirement | Value |
| --- | --- |
| Engine | Unreal Engine 5.8 |
| Shader model | SM6 (the lens passes are only compiled for SM6) |
| Tested platform | Windows, DirectX 12: the editor, Movie Render Queue and packaged games (Development) |
| Anti-aliasing | TSR or TAA recommended; the lens pass runs after them by default |
| Engine plugins used | Cinematic Camera (built in) |

- Without its `Binaries` folder, the plugin compiles on first launch, which needs a C++ project.
- No project setting is needed. The plugin registers its shaders at startup, so it must be enabled when the editor launches.
- Only views seen through a Lens Tracing camera are traced. Engine depth of field and motion blur are turned off on those views only: the lens tracer makes its own depth of field, and its own motion blur when `r.LensTracer.MotionBlur` is 1 (off by default for now).
- Lens lengths are in millimetres. Focus distances are in scene units, converted with the world's World to Meters.

## Technical limits

- Spherical, centred surfaces only: no aspheres, no anamorphic squeeze.
- Focusing moves the whole lens (the sensor), zooms included: no internal focusing. Between the positions a zoom lists, its moving gaps change in straight lines.
- Dispersion from each glass's refractive index and Abbe number only: no anomalous glass (fluorite, ED). The Lens Editor's drawing keeps a round iris.
- At most 32 rims and 1024 rays per pixel.
- Orthographic views are never traced.
- Silhouettes over strong blur can show a mixed-colour halo (no occlusion of the background).
- Motion blur has no occlusion either: an object moving over a still background keeps solid edges with faint wings, and a small moving light gains up to a third of its energy. Motion is a straight line during the exposure.
- Sensor noise: no hot pixels, row noise or Bayer colour pattern yet.
- Movie Render Queue crops the margin the lens renders for its distortion from the whole output frame: a camera letterboxed in another aspect ratio keeps it, smeared, at its sides, and with High Resolution tiles the frames whose enlarged size doesn't divide by the tile count come out uncropped (Overscan Percentage Override fixes it; the included lenses need at most 6 %). Its temporal samples mix both lenses on an Active Lens change and average the sensor noise; they don't add rays.
- Not validated yet: the motion blur (`r.LensTracer.MotionBlur 1`) in Movie Render Queue and while scrubbing Sequencer, chromatic aberration in Movie Render Queue, Movie Render Queue graph configs, Shipping builds and platforms other than Windows.

## Roadmap

What is planned. There are no dates, and the order can change: tell us what matters most for your shots (see *Need help?*).

**Being finished**

| Coming | What you get |
| --- | --- |
| Motion blur out of experimental | The lens tracer's own motion blur, made faster where moving objects cross depth edges, then shown on the camera again. |
| Faster chromatic aberration | Colour fringes at less than today's 2.5 times the cost, then a switch on the camera again. |

**Closer to a real camera**

| Coming | What you get |
| --- | --- |
| Special glass | Fluorite and ED elements keep their own colour behaviour: Tele_300mm loses the slight colour fringes it shows today. |
| T-stops | Light lost in the glass and at each surface: the exposure follows the lens's T-stop, as on cine lenses. |
| Aspherical surfaces | Imported lenses keep their aspheres, so their distortion and sharpness match the real lens. |
| Internal focusing | Lenses that focus with an inner group or floating elements breathe and focus close like the real ones. |
| Zoom viewpoint | The viewpoint drifts a few millimetres as you zoom, as on a real zoom. |
| See-through objects in focus | Glass, water and smoke focus at their own distance, with no material setting. |
| Sensor flaws | The colour blotches of a Bayer sensor in low light, hot pixels and row noise. |

**New looks**

| Coming | What you get |
| --- | --- |
| Flares and ghosts | Ghosts traced from the lens's own surfaces, coloured by their coatings; a starburst from the iris blades. |
| Anamorphic lenses | Oval bokeh and the horizontal squeeze. |
| Tilt-shift | Tilted and shifted elements: a tilted plane of focus. |
| Soap-bubble bokeh | The bright-rimmed bokeh of over-corrected lenses, and the smooth or harsh backgrounds that spherical aberration gives. |
| Diffraction | The softening of small apertures, as when you stop down to f/16 and beyond. |
| Aperture and shutter priority | The camera sets the shutter speed, or the f-number, from the meter, as on your camera. |
| Film look | Halation and film grain. |
| Fisheye lenses | Fisheyes wider than about 120°. |
| Smoother colour fringes | One wavelength per ray instead of three colour bands. |

**More accuracy and speed**

| Coming | What you get |
| --- | --- |
| Third-order optics | Spherical aberration and coma, and cat's eyes that stay exact at the corners of wide angles. |
| Cleaner edges | No halo around a sharp object in front of a very soft background, and a blurred or moving foreground that lets the background show through its edges. |
| Reference mode | An exact ray-by-ray render, to compare a shot against the truth. |
| Faster large bokeh | A gather pass and bokeh sprites for strong blur, and smoother large bokeh. |
