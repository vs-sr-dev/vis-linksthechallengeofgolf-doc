# 06 — the pictures: `GIFM` is a GIF, and the colour table is ten bytes from where it looks

*Measure: eight full-screen images, 320 × 200, 8-bit indexed, inside the
archives. The header matches the GIF logical screen descriptor field for field
and carries a real GIF image descriptor at offset 781. The colour table is at
**13**, not 23; the wrong reading renders every picture legibly in wrong
colours and scores **worse than a shuffled palette**. Eight of eight parse;
three negative controls refuse.*

---

## The format

Eight members named `.MLD` decode to exactly 64,791 bytes each. They open
`47 49 46 4d` = `GIFM`, and laid against the GIF specification the header
matches field for field:

    offset  size  GIF 87a/89a field            value here
    ------  ----  ---------------------------  -------------------------------
       0      6   signature + version          'GIFM' + 0x17 0x03
       6      2   logical screen width, u16    320
       8      2   logical screen height, u16   200
      10      1   packed                       0xD7 = table present,
                                               colour resolution 6 bits,
                                               not sorted, size 2^(7+1) = 256
      11      1   background colour index      0
      12      1   pixel aspect ratio           0
      13    768   global colour table          256 x RGB
     781      1   image separator              0x2C, which is ','
     782      2   image left, u16              0
     784      2   image top, u16               0
     786      2   image width, u16             320
     788      2   image height, u16            200
     790      1   packed                       0x00, no local table, no interlace
     791  w * h   image data                   RAW 8-bit indices

    13 + 768 + 10 + 320 x 200 = 64,791, on 8 of 8.

**The one departure from GIF is the last row, and it is the whole design.** A
GIF stores image data as LZW codes in sub-blocks. Here it is raw indices,
because the `MDmd` node containing the picture is already LZW-compressed by the
same algorithm GIF uses. The compression was hoisted out of the picture format
and into the archive. See [05-the-codec.md](05-the-codec.md).

An earlier draft of `mldpng.py` carried the sentence *"GIFM is not GIF; it
shares three letters and nothing else."* That was wrong, and it is a specific
failure worth naming: an abbreviation was expanded, then **disbelieved**, and
the disbelief was written down as though refusing a guess were the same as
making a measurement. It is not. `GIFM` had to be laid against the
specification before either answer was worth anything.

## The colour table, and how it was got wrong

Reading the first bytes as *4-byte magic, then a header length of `0x17` = 23*
instead of *6-byte GIF signature and version* puts the table ten bytes late.
Every image still renders. The geometry is right, the text is legible, and the
colours are wrong — which is exactly the trap
[`vis-sherlockholmes-doc/docs/05-imv-picture.md`](../../vis-sherlockholmes-doc/docs/05-imv-picture.md)
documents for this same platform, on a different disc and a different format.
Its method was reused here and its warning applies here.

The first attempt to settle it scored candidate readings by mean |ΔRGB| between
horizontally adjacent pixels, **without controls**, and produced a confident
ranking. That ranking was meaningless, and Sherlock's chapter says exactly why:
*a shuffled-palette control and an identity-greyscale control are scored
alongside, so the winning number has a scale.* Adding them:

    python tools/palscore.py _work/members/TORREY_P.CRS/0013_CRSVIEW.MLD \
        --windows --flat --extern _work/members/TORREY_P.CRS/0010_PALETTE.COL

    reading                          mean dRGB   vs ctrl
    window@13 RGB                        23.51     1.58x
    window@12 RGB                        28.41     1.31x
    window@14 RGB                        30.36     1.22x
    embedded@23 BGR                      38.10     0.97x

    CONTROL shuffled palette             37.12
    CONTROL identity greyscale           43.30

**The original reading scored 38.10 against a shuffled palette at 37.12: worse
than random.** Offset 13 wins at 1.58×, which is a thin margin and is reported
as thin. It was corroborated two other ways: structurally, because entry 0 at
offset 13 is `(0,0,0)` black and the entries after it are white, which is the
shape the artwork needs; and by the owner of this machine, who has independent
knowledge of what these screens look like and confirmed the render.

**The margin is thin for a measurable reason.** These pictures are dithered.
`palscore.py --flat` reports 20.51 % of 2 × 2 blocks uniform on the course
photograph, against Sherlock's range of 15.6–82.1 % for drawn art and 2.2 % for
continuous tone. Dithering deliberately alternates neighbouring pixels between
two entries, which raises the adjacency score for the *correct* palette too and
compresses the separation. A 2 × 2 box average of the same pixels looks like an
ordinary photograph.

## The colour tables are not all the same depth

| image | archive | table max | all multiples of 4 | indices used |
|---|---|---:|---|---:|
| `CDDSP.MLD` | `GRAPHICS.LZ` | 255 | no | 19 |
| `NUMPLRS.MLD` | `GRAPHICS.LZ` | 255 | no | 62 |
| `PLRENTER.MLD` | `GRAPHICS.LZ` | 255 | no | 40 |
| `SELPLAY.MLD` | `GRAPHICS.LZ` | 255 | no | 20 |
| `CDCHANGE.MLD` | `GRAPHICS.LZ` | 255 | no | 19 |
| `SC.MLD` | `SCORCARD.LZ` | 255 | no | 7 |
| `CRSVIEW.MLD` | `TORREY_P.CRS` | **252** | **yes** | 233 |
| `TOPMAP.MLD` | `TORREY_P.CRS` | **252** | **yes** | 167 |

The two course images carry 6-bit VGA values shifted into 8 bits — every byte a
multiple of 4, maximum 252 = 63 << 2. The six interface images carry genuine
8-bit values. **Both are written out unchanged**; `--vga6` exists for the disc's
two separate `.COL` members, which max at 63, and it fails loudly if handed
anything larger.

`PALETTE.COL` inside `TORREY_P.CRS` is a 768-byte 6-bit table. It is **not** the
course photograph's table: they agree on 57 bytes of 768. It belongs to the 3D
course renderer, and the photograph carries its own.

## What the eight pictures show

Six are the game's interface, and they are the evidence
[09-the-game-test.md](09-the-game-test.md) runs on.

**`SELPLAY.MLD`** — the mode menu, and G1 and G4 in one frame:

    PLAY      [ 18 Holes ] [ Front 9 ] [ Back 9 ]
    PRACTICE  [ 1 ][ 2 ][ 3 ] ... [ 17 ][ 18 ]
              [ Cancel ]

**`NUMPLRS.MLD`** — *"How Many Players?"* with buttons 1 to 6 Players and
Cancel, and two golfer figures in a preview panel.

**`SC.MLD`** — the scorecard: holes 1–9 with `Out` and `Tot`, holes 10–18 with
`In` and `Tot`, a `Par` row against every hole, and a `Cancel` button.

**`CDDSP.MLD`** — *"The Course on this CD-ROM is:"* / *"Do you wish to use this
course?"* with Yes, No, Cancel.

**`CDCHANGE.MLD`** — *"Please insert a new Course CD. Then select Continue."*
with Continue and Cancel.

Those last two are the most consequential thing in this chapter, and they are
not about pictures. **This is a 1992 console game whose shipped interface asks
the player to swap the disc for another content disc, reads what is on it, and
asks whether to use it.** The architecture was built and drawn; one course
shipped. See [03-the-medium.md](03-the-medium.md) for the arithmetic of what
would have fitted, and [10-against-the-collection.md](10-against-the-collection.md)
for what the MS-DOS release did with the same design.

**`PLRENTER.MLD`** is the name-entry screen; `GOLF.EXE` carries `ENTRNAME.BLK`
and `PLAYER 1` through `PLAYER 8` beside it.

Two are the course, and they are the only photographs on the disc:

**`TOPMAP.MLD`** — *TORREY PINES — SOUTH COURSE*, `PAR 72`, eighteen numbered
holes on a plan of the property, and three tee distances with their ratings:
7021 Yds (74.0), 6706 Yds (72.2), 6447 Yds.

**`CRSVIEW.MLD`** — an aerial photograph of the course with a caption rendered
into the image:

> Set atop the cliffs at the edge of the Pacific Ocean, the TORREY PINES 36
> hole course is one of the finest, most picturesque municipal golf facilities
> in the world. It has been a regular stop on the PGA TOUR since 1968, as the
> site of the SHEARSON LEHMAN HUTTON OPEN.

That caption was read out of the pixels, not out of a string table: it is drawn
text, and it became legible as soon as the picture was rendered against an
identity greyscale ramp — before the colour table was found at all.

## `.BLK`, which is 76 members and is not solved

The other picture-like category is `.BLK`: 76 members, sprites and panels. Its
header reads cleanly and its body does not close.

| member | u16 @0 | u16 @2 | what follows | w × h + 4 | actual length |
|---|---:|---:|---|---:|---:|
| `PLOTBALL.BLK` | 2 | 2 | `00 02` then two bytes, twice | 8 | **16** |
| `ROTATE.BLK` | 58 | 14 | `00 3a` — big-endian 58 = the width | 816 | **848** |
| `HELPBTNA.BLK` | 160 | 35 | `00 a0` — big-endian 160 = the width | 5,604 | **11,360** |
| `QUITCONF.BLK` | 86 | 59 | `00 56` — big-endian 86 = the width | 5,078 | **5,200** |
| `GOLFER.BLK` | 75 | 93 | a run of u16 LE values all equal to 75 | 6,979 | **41,472** |

Width and height are legible and a per-row prefix that repeats the width is
visible on three of five. **No arithmetic tested closes on any specimen**, and
`GOLFER.BLK` has a different shape again — plausibly several animation frames,
which `PUTTER`, `CHIPPER` and `GOLFER` being three members of a golfer archive
would support, and which is a reading and not a measurement.

The honest output is the table above and a refusal. Filed in
[13-leftovers.md](13-leftovers.md).
