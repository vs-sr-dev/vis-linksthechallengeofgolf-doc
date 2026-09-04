# 13 — leftovers: what is on the disc, described, and not explained

*Measure: everything in this chapter was measured and none of it was solved.
Each entry states what was tested and what the test returned, so that the next
person starts where this session stopped rather than where it started.*

---

## 1. `TITLE.SCR` — 270,098 bytes, six decodings tested, none of them it

The largest non-archive file on the disc and **11.82 % of its file bytes.** It
is the only file whose name promises a picture and whose picture did not come
out.

**What is measured.** The first eighteen bytes parse cleanly as a Truevision
Targa header and the fields are plausible:

    idlength 0   colourmaptype 1   imagetype 1
    cmap first 0   cmap length 0   cmap entry size 24
    x 0   y 0   width 640   height 400   bpp 8   descriptor 0x00

and are **internally inconsistent**: a colour-map entry size of 24 bits with a
declared colour-map length of 0 is not a Targa any writer produces.

**One piece of arithmetic closes exactly:**

    270,098 - 18  =  270,080  =  640 x 422

**And the geometry is confirmed by eye.** Rendered as raw 8-bit indices at 640
bytes per row starting at offset 18, the words **LINKS** and **VIS** appear in
correct letterforms at coherent positions, on a field of noise. Rendered
starting at 786 instead, the same words appear at incoherent positions. The
owner of this machine compared the two renders and confirmed which was which.
So **the data begins at 18 and the row stride is 640 bytes**, and the declared
height of 400 is not the number of rows present.

**Six readings were tested and all six failed, with their numbers:**

| reading | result |
|---|---|
| raw 8-bit indices, 640 × 422 | letters legible, everything else noise |
| Targa RLE (packets of count + data) | produced 300,055 pixels, needs 256,000 |
| Windows `BI_RLE8` | hit an end-of-bitmap escape after 2,192 of 270,080 bytes |
| the disc's own LZW (`mdmd.lzw_decode`) | code 488 out of range, 3 bytes out |
| FLI / FLC animation | magic `AF11`/`AF12` occurs **0 times** in the file |
| 8 bit-planes of 80 bytes per row | worse than the chunky reading; no structure |

**The palette was hunted and not found.** Eleven known palettes from this disc
(the eight `GIFM` colour tables, `TOPVIEW.COL`, `PALETTE.COL`) plus 196
768-byte windows from the file's own trailing region, scored by mean adjacent
|ΔRGB| against a shuffled-palette control and an identity-greyscale control.
**Best candidate 1.18× the tightest control** — below the 1.5× threshold
`palscore.py` treats as "does not know". A second hunt over 1,450 non-degenerate
768-byte windows of `GOLF.EXE`, scored scale-invariantly as adjacent distance
divided by random-pair distance, put the best at 0.714 against a shuffled
control at 0.942: **a factor of 1.32, also not a result.**

**One measured oddity for whoever picks this up.** The index histogram of the
raw reading is concentrated on multiples of 32 — 0, 32, 64, 96, 160, 192, 224
are seven of the ten commonest values, alongside 1, 2, 3, 29, 30, 31. That is
either a palette organised as eight ramps of thirty-two, or a sign that the
bytes are not palette indices at all. **This session could not tell which**, and
it is the first thing to settle.

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
