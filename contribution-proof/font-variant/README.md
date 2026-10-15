# Pillow font-variant proof - iAlturki

The requested engine is `ImageFont.Layout.BASIC` in both cases. The original
font uses `ImageFont.Layout.RAQM`. Before the fix, `font_variant()` keeps RAQM
because the enum value BASIC is zero. After the fix, it selects BASIC.

`before-after.png` contains actual output from the unchanged and fixed Pillow
implementations. No personal media or desktop captures are included.

At font size 54, the generated text `AVATAR office` has an advance of 368.28125
pixels before the fix and 385 pixels after it. A font constructed directly with
BASIC measures 385 pixels. The corrected rendering matches that reference pixel
for pixel. Measurements use FreeType 2.13.2 and Raqm 0.10.1; exact advances can
vary with library versions. Engine selection is the regression being fixed.

## Reproduce

From a Pillow checkout built with FreeType and Raqm, run the included script:

```sh
python3 /path/to/reproduce.py before
```

Run against unchanged source for the before capture and against the fixed
source for the after capture (`python3 /path/to/reproduce.py after`). The script
uses the checkout's bundled DejaVu Sans font, saves the actual rendered image,
compares it with a directly constructed BASIC font, and prints measurements.

Base revision: `27af434ef326fd5f71e600ba39c86c5706526fc3`.

The accompanying PR contains only the fix, tests and release note. This proof
is on a separate branch and adds no runtime dependency to Pillow.
