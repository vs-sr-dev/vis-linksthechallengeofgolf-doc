# 05 — the codec: LZW, found by known plaintext, and `.LZ` had nothing to do with it

*Measure: every compressed member of every archive is variable-width LZW,
LSB-first, initial width 9, first free code 258, clear at 256, end at 257,
ceiling 13 bits. **`--verify` decodes all 320 members and compares each against
the `raw` field in its own header: 320 of 320.** The identification did not come
from the file extension and the extension would not have supported it.*

---

## Why the extension was not evidence

Eight of the nine archives are named `.LZ`, and the pre-briefing said the
obvious thing about it and then said the necessary thing:

> The name `.LZ` suggests Lempel-Ziv and **the name of a file is not a
> measurement of its contents** — this repository has lost points on that exact
> move twice.

The ninth archive is `TORREY_P.CRS` and is byte-for-byte the same format, which
settles it from the other direction: **the extension does not determine the
format on this disc.** Had the codec turned out to be RLE, `.LZ` would have
been a marketing decision from 1990 and nothing more.

## What it actually was, and how

The break came from the container, not the codec. `SOUNDW.LZ` has 97 members
and **nineteen of them are stored verbatim** — the flag at +24 reads 0 and
`raw == stored`. All nineteen open `52 49 46 46` = `RIFF`.

So the first twelve bytes of the other seventy-eight were known before anything
was decoded: every one of them is a RIFF WAVE file and begins
`52 49 46 46 xx xx 00 00 57 41 56 45`.

`DING.WAV`'s stored stream begins:

    00 a5 24 31 42 b0 05 00 00 57 82 58 29 62 a6 0d

Read as **LSB-first bit codes of width 9**:

    bits from 0x00, 0xa5 ...   ->  256    a clear code
    next nine bits             ->   82    'R'
    next nine bits             ->   73    'I'

That is ordinary variable-width LZW as used by `compress(1)` and by GIF. No
other reading was needed and none was tried afterwards to make it fit.

## The one parameter that had to be searched

Everything except the code-width ceiling falls out of the standard scheme. The
ceiling was found by running the decoder at 12, 13 and 14 against `DING.WAV`'s
declared length of 11,598:

| ceiling | decoded bytes | verdict |
|---:|---:|---|
| 12 | 10,205 | truncates |
| **13** | **11,598** | **exact** |
| 14 | 11,598 | never reached, so indistinguishable from 13 |

13 is the value that makes the declared lengths come out right; 14 is not
excluded by this disc, because no stream on it ever needs a fourteenth bit.
That is stated rather than hidden: **`MAX_WIDTH = 13` is the smallest ceiling
consistent with 320 of 320, and the disc cannot distinguish it from a larger
one.**

## The check that makes it a measurement

The packer wrote `raw` into every node header. The decoder derives it again
from the stream. The two never see each other.

    python tools/mdmd.py _work/iso/*.LZ _work/iso/TORREY_P.CRS --verify

    GOLF1.LZ           9 of   9 members decode to their declared length
    GOLFER_F.LZ        3 of   3 members decode to their declared length
    GOLFER_M.LZ        3 of   3 members decode to their declared length
    GRAPHICS.LZ       14 of  14 members decode to their declared length
    LIE.LZ             6 of   6 members decode to their declared length
    SCORCARD.LZ        1 of   1 members decode to their declared length
    SOUNDW.LZ         97 of  97 members decode to their declared length
    TOPVIEW.LZ         3 of   3 members decode to their declared length
    TORREY_P.CRS     184 of 184 members decode to their declared length

    TOTAL 320 of 320

**A codec that were merely plausible would not survive 320 firings of a
quantity encoded twice.** And the outputs are independently checkable: 97 of
them parse as RIFF/WAVE with two agreeing duration routes, eight of them parse
as `GIFM` pictures whose declared dimensions predict their exact length, and
two of them are byte-identical to files in the MS-DOS release of the same game
decoded by the same decoder.

## Where the compression sits, which is the elegant part

The eight `GIFM` pictures inside these archives are **GIF files with their
image data left as raw palette indices** — no LZW, no sub-blocks. See
[06-the-pictures.md](06-the-pictures.md).

That is not an accident. GIF's own image codec is LSB-first variable-width LZW
with a clear code at 256 — **the same codec this container uses**. Access
Software took the compression out of the picture format and put it in the
archive, one level up, so that a picture and a `.WAV` and a terrain patch all
get compressed the same way and a picture is never compressed twice.

Reading it in that order also explains the container's numbers. `SOUNDW.LZ`
compresses to 0.8840 of its content because 8-bit PCM speech is nearly
incompressible by LZW; `GRAPHICS.LZ` compresses to 0.1244 because full-screen
palette artwork with flat regions is exactly what LZW is good at.

| archive | stored / raw | why |
|---|---:|---|
| `SCORCARD.LZ` | 0.0800 | one 320×200 screen, 7 of 256 colours used |
| `GRAPHICS.LZ` | 0.1244 | UI screens, 19–62 colours each |
| `GOLF1.LZ` | 0.1492 | flat panels and sprites |
| `TOPVIEW.LZ` | 0.2561 | a map |
| `LIE.LZ` | 0.4387 | lie tables |
| `GOLFER_F.LZ` | 0.5628 | golfer animation frames |
| `GOLFER_M.LZ` | 0.5762 | the same |
| `TORREY_P.CRS` | 0.6138 | terrain, plus one dithered photograph |
| `SOUNDW.LZ` | **0.8840** | recorded speech |

## What this does to a prediction

Clause **C21** of [00-predictions.md](00-predictions.md) reads, in full:

> **A working decompressor is not reached this session.** I predict the session
> identifies the container completely, extracts every member as a compressed
> blob with a correct length, and stops there, publishing a described shape and
> a stated refusal rather than a decoder.

It is wrong, and it is the most satisfying way to be wrong that this series
offers. It was written on the reasoning that identifying a 1990 compression
scheme from its output is expensive — which is true, and which is why the
method mattered: **the container handed over nineteen plaintexts before anyone
asked it to.**

The generalisable form, for the next disc: *when a container stores some
members compressed and some verbatim, the verbatim ones are a known-plaintext
attack that the format is giving you for free. Look for `raw == stored` before
you look at the bitstream.*
