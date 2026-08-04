# HLG to SDR/HDR — Release Notes

This file ships with every release (DMG and the public releases repository).
English is the reference text.

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
