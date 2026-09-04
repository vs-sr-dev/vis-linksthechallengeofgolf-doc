# 13 — leftovers: what is on the disc, described, and not explained

*Measure: everything in this chapter was measured and none of it was solved.
Each entry states what was tested and what the test returned, so that the next
person starts where this session stopped rather than where it started.*

---

## 1. `TITLE.SCR` — the picture is solved, the palette is not

The largest non-archive file on the disc and **11.82 % of its file bytes.**
**Its geometry, its content and its text are now measured. Only its colour
table is missing**, and this chapter is a record of where it is not.

### The layout, and it closes with residue 0

    18 bytes   header
  256,000      the picture: 640 x 400, one byte per pixel
   14,080      a second raster, 640 x 22
  ---------
  270,098      the file's own length

The header parses as a Truevision Targa header and its fields are read
straight: `idlength 0`, `colourmaptype 1`, `imagetype 1`, colour-map length
**0**, colour-map entry size 24, `x 0`, `y 0`, **width 640**, **height 400**,
**bpp 8**. A colour-map entry size of 24 bits with a declared length of 0 is
internally inconsistent for a real Targa — **and the declared 640 × 400 turns
out to be exactly right**, which the first pass missed by noticing only that
270,080 = 640 × 422 and concluding the height field was wrong.

**The extra 22 rows are not part of the picture.** They are a separate
640 × 22 strip holding one line of drawn text — `Press BUTTON (A) to Continue`
— which the game blits over the middle of the screen at run time. That is why
rendering all 422 rows as one image puts the prompt at the bottom where the
copyright line belongs. The owner of this machine caught it against his
knowledge of the running title screen, and the strip renders on its own and is
legible.

### What the picture contains

A coastal scene: sky, palm trees, a green, the **LINKS** logo top left, a
**VIS** logo bottom right in a box, and — drawn into the artwork at rows
366–394 — `(C) 1992 ACCESS SOFTWARE INC`. See
[12-privacy.md](12-privacy.md); it is the only year anywhere on this disc
outside a date field.

**Index 0 is the ink.** It is the only spatially coherent index in the file:

    solidity, the fraction of an index's pixels whose four neighbours
    all carry the same index

      index   0 : 0.320
      every other index : 0.002 or less, most of them 0.000

The letterforms of `LINKS` are index 0 at **299 of 300 pixels** sampled inside
a stroke. Everything else in the picture is dithered so finely that no pixel of
any other index has four matching neighbours.

### The row stride is 640 bytes, and that is proved rather than assumed

Rendered at 1,280 bytes per row the picture appears **twice, side by side**;
at 320 pixels of two bytes each it appears **twice, stacked**. Only 640 bytes
per row puts one copy on the screen, and the copyright line then spans x
0.039–0.727 of the width against 0.035–0.731 in the ground-truth screenshot.
**640 bytes per row, 400 rows.**

### Nine decodings tested and rejected, with their numbers

| reading | result |
|---|---|
| Targa RLE (count + data packets) | produced 300,055 pixels, needs 256,000 |
| Windows `BI_RLE8` | end-of-bitmap escape after 2,192 of 270,080 bytes |
| the disc's own LZW (`mdmd.lzw_decode`) | code 488 out of range, 3 bytes out |
| FLI / FLC animation | magic `AF11`/`AF12` occurs **0 times** |
| 8 bit-planes of 80 bytes per row | destroys the structure the chunky reading shows |
| cumulative sum along rows / columns / whole | local index diversity rises from 37.6 to 50.8–51.3 per 8 × 8 block: worse |
| XOR-cumulative along rows | 50.6: worse |
| 16-bit RGB555 / RGB565, LE and BE, at 320 × 400 | noise |
| 16-bit RGB555 / RGB565, LE and BE, at 640 × 200 | the picture appears twice |

The picture is **not compressed**: the shapes, the logos and both lines of text
are legible directly from the bytes at 640 per row. What the bytes *mean* is
the open question, and the next two sections are why.

### The byte is not a global palette index, and that is measured

The owner of this machine supplied a screenshot of the running title screen.
Aligned against the file — the `VIS` logo box sits at x 0.747–0.975, y
0.795–0.980 of the file and x 0.753–0.982, y 0.784–0.975 of the screenshot, so
the mapping is essentially 1:1 — it settles the question.

**Index 0 is the only value that forms solid regions**: 45 blocks of 8 × 8 are
90 % or more a single index, and every one of the 45 is index 0. Their
ground-truth colours are:

| where | ground truth |
|---|---|
| the `LINKS` logo, x 56–184, y 32–96 | **orange**, (101–128, 40–55, 0–2) |
| the logo's shadow, x 208–224, y 88–104 | **near-black**, (9, 2, 2) |
| x 48, y 152 | **black**, (0, 1, 2) |
| the `VIS` logo, y 320 | **bright blue**, (9, 60, 226) |
| the `VIS` logo, y 344 | **dark blue**, (36, 27, 122) |
| the `VIS` logo, y 368 | **magenta**, (129, 14, 57) |

**One byte value, five unrelated colours, in regions that are 90 % or more that
single value.** No palette assigns index 0 five colours. Whatever these bytes
are, they are not indices into one 256-entry table — which is also why the
searches in the next section were always going to fail, and why they are worth
reporting anyway: they close the door properly.

A least-squares fit of a 256-entry palette against the screenshot over 3,664
blocks of 8 × 8 — an over-determined system at 14.3 : 1 — leaves a residual of
**40.6 of 255 per channel**. A correct model would not.

### The invariant nobody had seen

**Every byte at an even column has bit 7 clear. All 128,000 of them.**

    even columns : 128,000 bytes, values 0..127, 128 distinct
    odd  columns : 128,000 bytes, values 0..255, 256 distinct
    mean |value(x) - value(x+1)| : 84      odd distances : ~89
    mean |value(x) - value(x+2)| : 70      even distances: ~73

The disc's known dithered photograph, `CRSVIEW.MLD`, shows no such parity
structure (42, 42, 44, 44, …). This one alternates by column parity at every
distance tested, and one of the two streams is missing a bit.

**That is the shape of two bytes per pixel with a 7 + 8 split**, at 320 pixels
per row — fifteen bits. The obvious readings of fifteen bits do not render (see
the table above), so the split is recorded as a measured invariant and not as
an interpretation. It is the sharpest single constraint anyone starting from
here will have, and it was found by counting rather than by looking.

### Where the palette is not

* **Not in `TITLE.SCR`.** The file is fully accounted for by header, picture and
  strip. Its longest run of bytes ≤ 63 — the range a 6-bit VGA DAC table would
  occupy — is **138 bytes**, so it holds no VGA palette anywhere.
* **Not in `GOLF.EXE`.** It has seven runs of ≥ 768 bytes all ≤ 63, at offsets
  4346, 11327, 12622, 13784, 26830, 42876 and 92707. **Every one of them is
  zero padding**: the largest, 48,327 bytes, has exactly one distinct value,
  and the others are 81–100 % zeros with at most ten distinct values. There is
  no 6-bit palette table in the program.
* **Not in the MS-DOS release either.** Of its 32 files, exactly one contains a
  non-trivial ≥ 768-byte run of 6-bit values — `title.lnx` at offset 30,745,
  770 bytes — and it is zero padding too: entry 0 reads (1,1,0) and entry 1
  (0,0,0), and it scores 0.918 against a shuffled control at 0.937.
* **Not any of the eleven palettes this disc does carry.** The eight `GIFM`
  colour tables, `TOPVIEW.COL` and `PALETTE.COL`, scored by 2 × 2 block
  smoothness against controls: best 0.8022 against a shuffled control at 0.9273
  and a greyscale control at 0.9120. **A factor of 1.16, which is not a
  result.**
* **And not anywhere else, exhaustively.** Every 768-byte window at stride 1 of
  `GOLF.EXE`, of `TITLE.SCR` itself, and of all 27 files over 1 KB in the
  MS-DOS release — **2,613,000 candidate windows** — scored by co-occurrence
  weighted colour distance normalised by the image's own colour spread, with
  windows rejected as degenerate unless the picture they render is as colourful
  as the disc's real palettes:

      best of 2,613,000              0.3169   GOLF.EXE @ 10023
      CONTROL identity greyscale     0.6387
      CONTROL shuffled palette       0.7918
      the disc's real palettes       0.8188 - 0.9270

  **That best score is a selection artefact and is reported as one.** The
  minimum of a noisy statistic over 2.6 million draws sits far below its mean
  by construction; the calibration case in the next section separates its one
  correct answer from a shuffled control among a handful of candidates, which
  is a different kind of number entirely.

### The statistic was calibrated before it was trusted

The measure used is the mean |ΔRGB| between horizontally adjacent 2 × 2 block
averages, divided by the same quantity over random block pairs — scale
invariant, so a flat table cannot win by being flat. It was calibrated on a
case where the answer is known:

| `CRSVIEW.MLD`, whose table is known to be at offset 13 | ratio |
|---|---:|
| the correct table | **0.2220** |
| the wrong table at offset 23 | 0.3807 |
| CONTROL — shuffled palette | 0.4186 |
| CONTROL — identity greyscale | 0.4420 |

**The statistic separates the right answer from a shuffled control by 1.89× on
a known case**, and it separates nothing on `TITLE.SCR`. That is the
difference between a tool that cannot tell and a tool that is telling you the
palette is not among the candidates.

### What is left for whoever picks this up

The palette is either computed by `GOLF.EXE` at run time, stored in a form no
byte-run test will find, or the picture is displayed through a table the
program builds rather than loads. The strongest anchor available is the ground
truth the owner of this machine supplied from the running game: **the `LINKS`
lettering is orange, the sky is light blue and lightens downward, and the bulk
of the image is blue sea and green.** Since the lettering is index 0, **palette
entry 0 is orange**, which is an unusual thing for entry 0 to be and is the
sharpest constraint anyone starting from here will have.

## 2. `.BLK`, 76 members, header legible and body not closing

The largest unopened category on the disc. Every specimen begins with two
little-endian u16s that read as width and height, and three of five follow them
with a big-endian u16 equal to the width — a per-row length prefix.

| member | w | h | what follows | w × h + 4 | actual |
|---|---:|---:|---|---:|---:|
| `PLOTBALL.BLK` | 2 | 2 | `00 02`, two bytes, twice | 8 | **16** |
| `ROTATE.BLK` | 58 | 14 | `00 3a` = 58 | 816 | **848** |
| `HELPBTNA.BLK` | 160 | 35 | `00 a0` = 160 | 5,604 | **11,360** |
| `QUITCONF.BLK` | 86 | 59 | `00 56` = 86 | 5,078 | **5,200** |
| `GOLFER.BLK` | 75 | 93 | a run of u16 LE all equal to 75 | 6,979 | **41,472** |

**No arithmetic tested closes on any specimen.** `HELPBTNA` overshoots by a
factor near two, which would fit a second plane — a mask, or a second colour
byte — and does not fit exactly. `GOLFER.BLK` has a different shape again, and
at 41,472 bytes for a 75 × 93 frame it holds roughly six such frames, which
would suit an animation and is a reading and not a measurement.

## 3. Sector 1667

509 zero sectors before it, seven after, no file within 500 sectors. **550
non-zero bytes beginning at user offset 1,496** — a tail in the last quarter
with nothing above it.

What is new this session is *why* it is anomalous. It is the **only** sector in
the image with a non-zero reserved field (`48 64 36 ab 56 ff 7e c0` at
2068..2075, where Mode 1 requires zero) and the **only** one whose P and Q
parity both fail. All three anomalies are the same sector.

So it is not a Mode 1 sector with odd contents; **its error-correction region
holds something that is not error correction**, and the non-zero run continues
from the user area into the reserved field without a boundary.

Tested and excluded: it is not the tail of any of the thirteen files (the last
ends at LBA 1157); it does not decode under the disc's LZW; it contains no
printable run of six characters or more; it is not a directory record.

The reading — that it is residue from the mastering system's buffer — is a
reading. **Nobody has explained it and this session did not either.**

## 4. `MD20` in sector 15

`4d 44 32 30`, 15 non-zero bytes in the first 37 of a sector in the system
area, where a mastering tool puts its own structures. `MD` matches
`MERIDIAN_DATA_CD_PUBLISHER` in the preparer field.

**It has nothing to do with `MDmd`**, and the dates prove it rather than the
intuition: two of the nine `MDmd` archives were built in 1990 and 1991 and are
byte-identical to files in the MS-DOS release, so their container predates this
disc's mastering by up to two years and cannot have been written by the CD
publisher that stamped sector 15 in October 1992. A shared two-character prefix
is not a lineage.

**What `MD20` contains is not identified.** Fifteen bytes.

## 5. `TEXT` and `hscd` in the root directory's System Use area

Present at LBA 34, in the field where Apple's ISO 9660 extensions live. Nothing
on this disc reads them and a Tandy console has no use for them. Inherited
furniture from the mastering software; unexamined beyond their presence.

## 6. The four run-out sectors

1,675 present, 1,671 declared, LBA 1671..1674 all zero. Ordinary track run-out,
noted for completeness.

## 7. `UUdebug.dmp`

A debug artefact name in a shipped retail program's string table, alongside
`Thinking...` and `Please stand by...`. Nothing on the disc is called that and
nothing writes it — this is a read-only medium. It is a leftover in the
original sense: a development path that survived to master.

## 8. `BRIAN.WAV`'s missing pad byte

97 of 97 WAVs parse; **one has a chunk walk that reaches 2,082 bytes in a
2,081-byte file.** RIFF requires odd-length chunks to be padded to even, and
this one was not. It is a defect in a 1990-vintage file, faithfully carried
into a 1992 pressing and faithfully preserved by the decompressor.

## 9. `GOLF1.LZ`, the `.LZ` with a digit and no sibling

There is no `GOLF2.LZ` on this disc — nor in the MS-DOS release, which also
ships exactly one `golf1.lz`. The name has never had a partner. It is not a
mystery so much as a naming convention that never needed a second member, and
it is recorded because the pre-briefing flagged it and it deserved an answer.

## 10. The nine unidentified bytes at +32 of every `MDmd` header

Every one of the 327 nodes carries nine bytes at offset 32 that were not
identified. Measured on `TORREY_P.CRS`, which has 185 of them:

    distinct values among 185 nodes : 182

so the field is **very nearly unique per node**. Three nodes share
`00 20 00 b5 a5 44 15 59 44` and two share a value one byte away from it, which
is what a checksum or a timestamp of files written seconds apart would look
like. The first three bytes are frequently `00 20 00` and the remaining six
carry the variation.

**No interpretation was tested**, so nothing is excluded: it could be a
checksum, a date, or a build counter. It is the largest wholly unexamined field
in the container and it is nine bytes wide.

## 11. `COURSE.HDR` and `TOPVIEW.DAT`

4,030 and 224 bytes, one of each, inside `TORREY_P.CRS`. `TOPVIEW.DAT` is
**stored uncompressed** and is therefore fully visible; neither was parsed.
They are the course's own metadata and the obvious next thing to read after
`.PAT`.

## 12. The 135 `.PAT` members

The largest single category on the disc by count, and the naming is a
**coordinate grid** rather than a sequence:

    first character  : 0 1 2 3 4 5 6 7 8 9 A B C D E F G   (17 values)
    second character : 0 1 2 3 4 5 6 7 8                   (9 values)
    17 x 9 = 153 cells, 135 present, 18 absent

and the eighteen absent cells are `00 01 02 08 10 11 20 30` and
`C8 D8 E8 F8 G0 G1 G2 G3 G4 G8` — **corners**. A rectangular grid with its
corners cut away is the shape of a golf course laid over a bounding box, and
`GOLF.EXE` carrying the literal template `PATCHxx.PAT` says the program
addresses these by coordinate rather than by index.

**Not one was parsed.** Together with the `.BLK` sprites they are what a
renderer for this game would need next.

## 13. `minalloc 0` / `maxalloc 1`

Recorded by the pre-briefing as a leftover and **resolved** here, so it is
listed only to close it: it is not a packer and not an overlay scheme. See
[08-the-executable.md](08-the-executable.md). The program checks for 530 K free
and refuses; it does not ask DOS for the largest block.

## 14. The 545 zero sectors

A third of the disc, and a decision somebody made. There is nothing in them —
that was checked, twice, by two tools with different definitions that
reconciled at 571. What they mean belongs to
[03-the-medium.md](03-the-medium.md), which works out that 866 more golf
courses would have fitted.
