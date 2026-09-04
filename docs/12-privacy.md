# 12 — privacy and provenance: three corporate names, one first name in a filename, and no user state

*Measure: 270 printable runs in `GOLF.EXE` read in full, 320 archive members
decoded and their names enumerated, 97 audio clips checked, eight pictures
rendered and read. **`protscan.py` reports 0 on all eleven markers over 15
files (6,224,038 B) and again over 320 members (3,069,433 B)**, with its
positive control firing on both passes. `LINKS.CFG` compared byte for byte
against the MS-DOS release before P.4 was asserted.*

---

## P.1 — the criterion, unchanged

> A name its owner put inside a product they made and sold to the public is
> published; a pseudonym a third party put inside a document that is not the
> product is not.

## P.2 — the corporate names, and they are published

| name | where | offset |
|---|---|---|
| `TANDY` | ISO 9660 publisher field | volume descriptor, LBA 16 |
| `MERIDIAN_DATA_CD_PUBLISHER` | ISO 9660 preparer field | volume descriptor, LBA 16 |
| `Tandy Corporation` | `CONTROL.TAT` copyright line | 0x000 |
| `Access Software Inc.` | `CONTROL.TAT` title field | 0x054 |
| `Maketat` | `CONTROL.TAT` build string | 0x1AF |

All five are P.1 publications: a manufacturer, a mastering house, a developer,
and a tool. Nothing here needs redacting and nothing was.

Two further corporate names appear **inside a picture** rather than in a
string table, and were read out of the rendered pixels of `CRSVIEW.MLD`:
**`PGA TOUR`** and **`SHEARSON LEHMAN HUTTON OPEN`**. Both are organisations,
both are drawn into artwork Access Software shipped, both are P.1.

**And a sixth, which is the only year on the disc outside a date field.**
`TITLE.SCR` carries, drawn into the artwork at rows 366–394 of its 640 × 400
image and legible once the low five bits of each byte are rendered as
luminance:

    (C) 1992 ACCESS SOFTWARE INC

That is worth putting beside the finding in
[08-the-executable.md](08-the-executable.md) that `GOLF.EXE` contains **no
`Copyright`, no `Access`, and no match for `19[0-9][0-9]` in 157,780 bytes**.
The program carries no attribution at all; the title screen carries it in
pixels. Both statements are measurements and they are not in tension — this is
simply where this developer put the credit.

The same file carries one more line of drawn text, in a 640 × 22 strip stored
after the image: **`Press BUTTON (A) to Continue`**. It names the console's
controller and it is the only reference to VIS input hardware found anywhere on
the disc.

## P.3 — personal names, which is not "none"

The last five sessions that predicted *no personal names* were wrong twice, and
this disc was expected to be the vintage that carries a credit string. Clause
**C30** predicted at least one personal name in `GOLF.EXE`'s strings.

**It is wrong.** All 270 printable runs were read and nineteen compiler,
vendor and copyright needles counted; every one is zero, and the regular
expression `19[0-9][0-9]` matches **zero times in 157,780 bytes**. There is no
credits string, no developer name, no year, and no copyright notice anywhere in
the program. The only names in `GOLF.EXE` are `PLAYER 1` through `PLAYER 8`.

**One personal name exists on the disc, and it is a filename.**

    SOUNDW.LZ > BRIAN.WAV     2,081 bytes, 1.24 s, 11,025 Hz mono

`BRIAN` is a given name, it is inside a product Access Software made and Tandy
sold, and it is therefore P.1 published material. It is recorded here rather
than redacted, at exactly the granularity at which it exists.

**And it is recorded with what it is not.** The owner of this machine played
the file: it is the sound of a club striking a ball. **There is no voice in it
and no name spoken.** Whoever Brian was — a developer, a colleague who swung a
club into a microphone, a joke that survived to master — the disc does not say
and this repository does not guess.

## P.3, second half — the golfers, the course, and the tournament

The pre-briefing raised a specific nuance: *the name of a real golfer licensed
into a 1990 product is the name of a real person*, and should be treated as
P.1 territory rather than waved through as fiction.

**No golfer's name appears anywhere on this disc.** Not in the executable's 270
runs, not among the 318 distinct member names, not in the rendered text of the
eight pictures. The commentary clips are named for events (`ACE`, `GRETEAGL`,
`INSAND`) and for the gender of the voice, never for a person.

What does appear is a **real place** — Torrey Pines South Course, San Diego —
with its real par, real yardages and real course ratings, and a **real
tournament**, the Shearson Lehman Hutton Open, named in drawn text inside
`CRSVIEW.MLD`. Places and tournaments are not personal data. They are recorded
in full in [06-the-pictures.md](06-the-pictures.md).

## P.4 — no user state, and this time it was checked

`LINKS.CFG` is 14 bytes and its name says configuration, so it was the one file
on the disc that could plausibly have held a saved setting. Fourteen bytes are
small enough to publish entire:

    VIS  LINKS.CFG :  00 01 00 01 07 00 20 02 03 80 25 00 01 08
    PC   links.cfg :  00 01 00 00 07 00 20 02 07 80 25 00 01 08
                                  ^^                ^^
                            byte 3               byte 8

**Twelve of the fourteen bytes are identical to the MS-DOS release's file of
the same name.** Two differ, and they differ the way a platform field differs:
byte 3 reads `0x01` here and `0x00` there, byte 8 reads `0x03` here and `0x07`
there. This is a **shipped configuration record pressed identically on every
copy**, not a setting somebody saved.

That is the whole of the candidate. The other twelve files are an executable, a
raster, a vendor block and nine archives whose 320 members are terrain, art,
sound and tables. **There is no user state on this disc**, and the sentence is
written after the check rather than before it.

For contrast, and to show what user state on this game actually looks like: the
MS-DOS release ships `default.plr`, `lexa.plr` and `lion.plr`, **548 bytes
each**, with an eighteen-byte space-padded name field at offset 2 —

    00 01 | 'DEFAULT           ' | 01 01 01 03 09 0a 0b 0c ...
    00 01 | 'LEXA              ' | 01 01 01 03 09 0a 0b 0c ...
    00 01 | 'LION              ' | 01 01 01 03 09 0a 0b 0c ...

— followed by an identical settings tail on all three. **Those are player
records carrying names somebody typed**, on somebody's copy of the game, and
they are the next repository's problem rather than this one's. `LEXA` and
`LION` read as handles rather than as legal names, which is a reading and not a
measurement, and the point of noting it here is only that **this disc has
nothing of the kind at all** — no `.plr`, no name field, no writable file.

## P.5 — provenance, and it is weak

The `.bin` and `.cue` carry a filesystem mtime of **1996-12-24**. That is
neither the mastering date nor the dump date; it is somebody else's timestamp
from somebody else's machine and it says nothing about this object.

The dates that mean something are inside the disc:

| date | source | what it dates |
|---|---|---|
| 1992-10-23 20:42:28 | primary volume descriptor | the mastering |
| 1992-10-22 16:50:58 | ISO 9660 record for `GOLF.EXE` | the file being written to the image |
| `10-22-92 at 16:46` | a string inside `GOLF.EXE` | **the program being linked** |
| 1992-08-14 15:24:26 | ISO 9660 record for `TORREY_P.CRS` | the course archive |
| 1992-07-01 16:56:36 | ISO 9660 record for `SOUNDW.LZ` | the sound archive |
| **1991-02-13 08:11:28** | ISO 9660 record for `LIE.LZ` | a file from the MS-DOS product |
| **1990-10-09 14:45:28** | ISO 9660 record for `TOPVIEW.LZ` | a file from the MS-DOS product |
| `1(12) 31-Aug-92` | `Maketat` string in `CONTROL.TAT` | the Tandy tool's own build |

**Two of those are corroborated by bytes rather than believed.** `LIE.LZ` and
`TOPVIEW.LZ` are byte-identical to the MS-DOS release's files of the same name,
so their 1990 and 1991 dates are not merely plausible: the files really are the
older product's.

And the link stamp and the ISO date on `GOLF.EXE` are **four minutes apart**,
which is two independent clocks agreeing — a build at 16:46 and a master image
written at 16:50:58, the same day.

**The `Maketat` build predates the mastering by 53 days.** `1(12) 31-Aug-92` on
a disc mastered 1992-10-23, where *Fitness Partner* already carries `1(13)
9-Oct-92`. **The Maketat build string does not track the mastering date**; it
dates the tool, not the pressing. That was a half-clause prediction and it
holds on the sixth disc.

## P.6 — the protection scan, on its home ground

A 1992 CD-ROM is what `protscan.py` was written for, and for once it had a real
target rather than an Android tree.

**The tool carries eleven markers**, counted out of its `MARKERS` list at lines
23–33, plus one positive control. The brief handed to this session said *nine*;
the brief before it said *eleven*. **Eleven is right**, and the instruction that
mattered was to count them rather than inherit the number.

    BoG_        SafeDisc          SECUROM       securom     CMS16.DLL
    LaserLok    CDCOPS            StarForce     TAGES       SETTEC
    Macrovision

Two passes:

| pass | files | bytes | marker hits | positive control |
|---|---:|---:|---:|---:|
| the disc's files, `--all-files` | **15** | 6,224,038 | **0 on all eleven** | fires on 13 |
| every decoded archive member | **320** | 3,069,433 | **0 on all eleven** | fires on 94 |

**And the first run of it was wrong, which is the point of reporting the file
count.** Run without `--all-files`, the tool reported `files searched: 2`. Its
`EXTS` filter admits `.exe`, `.dll`, `.scr` and friends, so on this disc it
opened `GOLF.EXE` and `TITLE.SCR` and skipped the nine archives, the vendor
block and the image — **and it would still have printed a table of zeroes.**

That is the fourth lesson from the previous session firing exactly as
predicted: *a reader that filters by extension instead of by what the format
declares does not say zero.* The zero above is over 15 files and then over 320
members, and both counts are published because a zero without a denominator is
not a measurement.

## P.7 — no execution, no emulation, no network

Nothing on this disc was run. There is no emulator in this pipeline; the disc
was read, its containers were parsed, its members were decompressed in Python,
and its pictures were converted to PNG with `zlib` from the standard library.
No byte of the product is published in this repository — hashes yes, bytes no.

The one human step was exactly that: a **human** step. The owner of this machine
played extracted audio and looked at rendered images, and reported what he
heard and saw. Three findings in this repository rest partly on that and say so
where they do: the identification of `DING.WAV`, the `F`/`M` naming rule, and
the colour table at offset 13.
