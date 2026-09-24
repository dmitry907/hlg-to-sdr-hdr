# HLG to SDR/HDR — Release Notes

This file ships with every release (DMG and the public releases repository).
English is the reference text.

## 0.791 — auto levels per photo is a switch

**The short version.** Each photo now has its own Auto levels switch instead of a one-shot
Auto button, and the batch switch is named for what it is.

- **Batch settings** (was «Settings for all photos») holds **Batch auto levels**.
- **Each photo has its own Auto levels switch.** It shows the batch setting until you flip it;
  flipping it changes that photo only, and the file list marks the photo.
- **Flipping Batch auto levels, either way, returns every photo to it** — photos with their
  own switch and manual ones included. Changing the preset does not.
- **Apply to all switches Batch auto levels off**: after it every photo carries the copied
  numbers, and a switch left on would say the opposite of what the files get.

## 0.79 — one set of settings for the whole batch

**The short version.** The window is reorganised around one rule: what applies to every
photo is set once, what applies to one photo is set on that photo, and the app shows which
is which. Closing the window now quits the app. Two texts that were wrong are corrected.

### Settings for all photos

Preset, format, quality, **Auto levels** and destination sit in one block and apply to the
whole batch. Before, the preset belonged to each photo, and because the formats on offer
depend on the preset, the format list changed with whichever photo was selected — selecting
a BT.2446 photo could quietly switch an HDR format back to SDR for everyone.

**Auto levels** is a switch. Off, photos are converted as the preset renders them, every
slider at zero. On, every photo is measured on its own and gets its own exposure, highlights
and shadows, and the sliders show what was measured for the selected photo — the preview is
what the file will be. Before, the switch acted only while converting: the preview showed
one thing and the file got another. If it was on in 0.78, it stays on.

### Settings for the selected photo

The sliders, **Auto**, **Reset** and **Apply to all**. Touching any slider, or pressing Auto
for that photo, makes the photo **manual**: it keeps its own numbers whether Auto levels is
on or off, and the file list marks it. Reset returns it to the settings for all photos.
Apply to all copies the selected photo's sliders to every photo and makes them all manual.

Before, a batch with Auto levels on overwrote numbers set by hand — including the ones Apply
to all had just copied — without saying so. That no longer happens.

### Closing the window quits the app

It used to keep running without a window, and macOS 26 then listed it under Login Items as
running in the background, although nothing in the app asks for that. Closing the window in
the middle of a conversion now stops the batch; every file already written is complete.

### Corrections

- The note on the HEIF format said HEIF is smaller than JPEG. It is larger at the same
  quality: on the test frames about 1.5× at 8 bit and about 2× at 10 bit. Corrected in all
  five languages.
- «Report a problem…» in About wrote to an address that did not exist, so reports went
  nowhere. It now writes to publicone@me.com.
- «Folder» is now called «Destination».

## 0.78 — two presets, honest sliders, twice the speed

**The short version.** Two of the four tone-mapping presets are gone, because on
inspection they were not what their names promised. The shadow and highlight
sliders no longer change one part of a picture differently from another. HDR
output is now offered only where it does something. Conversion is about twice as
fast. Batches can now set exposure for each photo on its own.

### Why Reinhard and ACES were removed

This is the change most likely to annoy someone, so here is the whole reasoning.

**"Filmic / ACES" was not ACES.** Under the name sat a five-constant rational
curve — a well-known approximation, published by Krzysztof Narkowicz in 2016, that
imitates the *shape* of the ACES RRT+ODT for real-time engines. Actual ACES is a
colour-management pipeline: input transforms, its own working primaries, a
reference rendering transform and an output transform. None of that was present,
and the approximation was fitted for scene-referred ACES values while this app fed
it display light normalised to its own anchor — outside the range it was fitted
for. What it did was apply a pleasant S-curve with a shoulder. That is a fine thing
to do; calling it ACES was not, and ACES is a trademark of the Academy. Keeping a
borrowed name on a curve that isn't the thing is the sort of claim that gets found
out, and the honest fix is to stop making it.

**"Reinhard" was the base preset without its lift.** Flat runs the extended
Reinhard curve with a white point and adds a gentle lift; the Reinhard preset ran
the same curve without the lift. One operator, two menu entries, presented as two
different approaches. Nothing was lost by removing it that a small exposure change
does not recover.

**What that leaves is one of each kind, on purpose.** Flat is a house look, built
to keep the most highlight detail and to be finished elsewhere. BT.2446 Method A
is the ITU standard, implemented from the report, chroma correction included, and
calibrated in absolute luminance. A look and a reference. The two that went were a
duplicate and a misnomer.

### Shadows and highlights are a global curve now

They used to work locally, the way Photoshop's Shadow/Highlight does: each pixel
was judged against a blurred version of its surroundings, so a bright pixel inside
a dark area still counted as shadow. That preserves fine texture beautifully, and
it also means two pixels of the same tone come out at different brightness
depending on what is around them. At strong settings that reads as a glow along
high-contrast edges — a bright seam between a dark shirt and a lit leg, for
instance. On a control frame, one and the same input tone came out 108 levels
brighter inside a dark neighbourhood and 8 levels brighter inside a bright one.

The sliders now read only the pixel's own brightness. Same tone in, same tone out,
wherever it sits. The price is real and worth stating: at full deflection the
shadow slider lifts about a third less than it used to, because a curve that stays
monotonic cannot do more without turning into a general brightness control, and
highlight recovery keeps less fine sky texture than a local operator can. The one
control that still looks at a pixel's surroundings is Highlight local contrast,
which is what it is for, and only when you turn it up.

### HDR output is offered with Flat only

The gain map records how hard the tone curve squeezed each pixel, so an HDR display
can let it back out. BT.2446 is calibrated absolutely and compresses very little in
normalised terms, so its map came out empty across more than 99% of a frame: the
file was larger, announced itself as HDR, and looked identical on an HDR screen and
an ordinary one. Rather than ship that, the HDR containers now appear only under
Flat, whose map is full.

### Also in this release

- **Roughly twice as fast.** The renderer now uses every core. Output is unchanged
  down to the byte — the dithering is reproduced exactly, which is checked on every
  build against reference renders.
- **Auto levels for a whole batch.** A switch in the output block measures each
  photo in the queue separately as it converts, so a card of mixed exposures comes
  out even. Distinct from "Apply to all", which copies one photo's settings to the
  others.
- **Settings are remembered** between launches: format, bit depth, quality, output
  folder and the preset a new photo starts on.
- **The preset block folds away**, and "Apply to all" now sits under the file list
  where the queue is.

### If you already use 0.777

Edits saved in Photos with Reinhard or Filmic/ACES still open — they come back on
Flat, the nearest surviving look, rather than failing to open. Pictures already
converted are unaffected: this changes the app, not your files. If you had settled
on the Reinhard preset, Flat with exposure a little lower is the closest match.

## 0.777 — first public release

**What it is.** A free macOS app + Photos editing extension that converts HLG
HEIF photos (Sony; Nikon Z8/Z9/Zf write the same format) into an honest SDR —
highlights keep their texture instead of burning to white — or into an HDR JPEG
with an ISO 21496-1 gain map that shows your grade on any screen.

**Highlights**

- Four tone-mapping operators (Flat/Linear, Reinhard, Filmic/ACES, BT.2446 A)
  with per-operator Auto, local shadows/highlights, live A/B against the system
  rendering or a reference file, 100% loupe, clipping-aware histogram.
- Output: JPEG or HEIF, 8/10-bit, SDR or HDR (ISO gain map), with a quality
  control. EXIF, orientation and nearly all XMP survive the trip.
- Photos editing extension: the same engine inside the Photos edit view, working
  on the HLG master rather than on a frame already folded down to SDR. Choose
  the container, the base depth and the quality that land in your library, or
  export the full-size result straight to a file without touching the library.
  Non-destructive throughout: reopen the edit later and every setting is where
  you left it, and Revert to Original always brings the master back.
- Interface in English, Russian, Japanese, Simplified Chinese and Spanish.

**Known limitations**

- The five interface translations are the author's own and have not been
  proofread by native speakers. The app is free — please don't look the gift
  horse in the mouth. If a translation makes you wince, the "Report a problem"
  button in About cheerfully accepts such reports, and fixes are quick.
- Nikon HLG HEIF should convert identically to Sony, but has not yet been
  verified on real Nikon files — judge results critically.
- Canon HDR PQ HEIF (a different transfer curve) and Panasonic HLG Photo
  (.HSP container) are not supported.
- Apple TV does not display HDR photos — anyone's; that is a tvOS limitation,
  not a defect of the files this app writes.
