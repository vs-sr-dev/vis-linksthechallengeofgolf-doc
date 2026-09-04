# 00 — predictions: nine sealed archives, one DOS binary, and a criterion that cannot be run as written

*Measure: every clause below was written before the measurement that scores it,
with the single, declared exception of §A.2, which records what was measured
first and is worth zero points. Clause count and score totals are produced by
`tools/checkscore.py`, not by hand.*

This is the fifty-first object in this series and the third VIS disc. The rule
is the same as the previous fifty: predictions are written down before the
object is opened, they are scored honestly afterwards, and the wrong ones stay
on the page.

---

## §A — the pre-briefing, which is worth zero points

Everything in this section arrived in `_pre\` or in `prompt.txt` and was
written by a previous session. **It scores nothing.** It is reproduced so that
the clauses in §B and §C can be read against what was already on the table.

### §A.1 — what the pre-briefing asserts

**The medium.** A single-track MODE1/2352 image, 3,939,600 bytes, 1,675
sectors, an exact divisor. Primary volume descriptor at sector 16 declaring
volume `LINKSVIS`, 1,671 logical blocks, publisher `TANDY`, preparer
`MERIDIAN_DATA_CD_PUBLISHER`, created 1992-10-23 20:42:28. Residue of 4
sectors of run-out; the image is complete and the disc is genuinely this
small. 3,430,400 bytes of user data, which is 0.55 % of a CD-ROM.

**The tree.** Thirteen files, zero directories, 2,284,337 file bytes, 1,123
file sectors. Two files carry pre-VIS ISO dates: `TOPVIEW.LZ` 1990-10-09 and
`LIE.LZ` 1991-02-13.

**The sector census.** 1,675 present, 1,671 declared, 1,123 covered by files,
552 covered by nothing, of which 545 all-zero and 7 not: sectors 15, 16, 17,
18, 26, 34 and 1667. Sector 15 opens `MD20`. Sector 1667 holds 550 non-zero
bytes whose first is at offset 1,496. The pre-briefing says explicitly that
this list came from one pass and must be re-run.

**The container.** Nine of thirteen files open `MDmd`; all eight `.LZ` and
`TORREY_P.CRS`. 1,855,971 bytes, 81.2477 % of the file bytes. Constant bytes
`0a 01 7a 00` at +4 on 9 of 9. A count byte at +10. A flag at +24 that is `01`
on the two pre-1992 files and `00` on the seven 1992 files. A three-byte value
at +25 equal to the count byte × 17 on 7 of 7. Member names in clear, 8.3,
space-padded. The pre-briefing calls the 17-byte stride *an inference and not a
measurement*.

**The program.** `GOLF.EXE`, 157,780 bytes, MZ, `nblocks` 309, `lastsize` 84,
`hdrsize` 96 paragraphs, `nreloc` 268, `relocpos` 0x1C, `cs:ip` 16AE:553E,
`ss:sp` 165A:0540, `minalloc` 0, `maxalloc` 1. `(309−1) × 512 + 84 = 157,780`.
No `e_lfanew`; the field at 0x3C reads 380,509,147 because the relocation table
begins at 0x1C. 270 printable runs of six characters or more, of which nine are
transcribed. `$`-terminated strings.

**The vendor block.** `controltat.py`'s full output, transcribed in
`_pre\formats.txt`, including the md5 of the leading 84 bytes matching the
platform notes.

**The neighbours.** The thesis figures 96.4344 % (*Sherlock Holmes*) and
95.8047 % (*Race the Clock*), and the G1..G4 test from `vis-gamelist-doc`.

**The three corporate names.** `TANDY`, `MERIDIAN_DATA_CD_PUBLISHER`,
`Access Software Inc.`

### §A.2 — what this session measured before writing these predictions

The calibration prescription carried into this session permits opening one
specimen of a sealed container before writing predictions, on the grounds that
opening one is a measurement and predictions are not scored against
measurements already made. That permission was used. **The following are
measurements, made before this file was written, and they are worth zero
points. No clause below is scored against any of them.**

1. **`tools/iso9660.py --extract` produced 13 files and 2,284,337 bytes**,
   agreeing with the pre-briefing's table, on the first run and without
   modification.

2. **`tools/controltat.py` was re-run on the extracted `CONTROL.TAT`** — the
   brief's order-zero command. The md5 of the leading 84 bytes is
   `ed9bfc904220e409f04c0772f1797ff7` and the tool reports `MATCH` against the
   platform notes' recorded value. `fdiv` is present at 0x0B0. The transcript
   in `_pre\formats.txt` is **confirmed**, with one detail it lost: the
   authorisation statement contains a `\xa0` byte between `System` and `Title`,
   which the transcript rendered as a line break.

3. **Four `MDmd` headers were dumped**: `SCORCARD.LZ`, `GOLF1.LZ` (1992
   variant) and `TOPVIEW.LZ`, `LIE.LZ` (pre-1992 variant).

4. **The 17-byte stride is a measurement now, not an inference.** In
   `GOLF1.LZ` the region beginning at file offset 122 reads as consecutive
   records of **13 bytes of NUL-padded name followed by a 4-byte little-endian
   offset**: `WNDARROW.BLK` → 275, `ADRESIND.BLK` → 739, `BALLMRKR.BLK` → 987,
   `SPLASH.BLK` → 1,207, `MAINPAN2.BLK` → 1,709, `DROP2.BLK` → 5,609,
   `PLOTBALL.BLK` → 9,272, `ROTATE.BLK` → 9,408. 13 + 4 = 17. The count byte is
   9, the directory length is 153, the directory begins at 122, and
   122 + 153 = **275**, which is the first member's declared offset.

5. **`DROP2.BLK`, the string inside `GOLF.EXE`, is a member of `GOLF1.LZ`.**
   The cheapest cross-check on the disc, which the pre-briefing set up and did
   not run, resolves in the affirmative. This is measured; §C does not claim it.

6. **The two pre-1992 archives carry build-machine paths in clear**:
   `C:\LINKS\TOPVIEW\` in `TOPVIEW.LZ` and `C:\LINKS\SOUNDS\` in `LIE.LZ`,
   stored as a length-prefixed string. `SCORCARD.LZ` carries a nested `MDmd`
   whose path field reads `..\ANI\`.

7. **The preamble of both variants ends at offset 122.** The 1992 variant
   places its name-length byte at 41 and its name at 42; the older variant
   places them at 42 and 43.

8. **The collection denominators were re-measured**: **112 directories in the
   root, 79 with a `notes\`**, this repository included and counted before it
   had one.

9. **Three repositories named in the brief are not where the brief says.**
   `vis-wolf3d\` and `vis-synth\` do not exist under that name anywhere on
   this machine; `vis-fileviewer` is not in `Homebrew7\` but one root over.
   This is a correction to the brief, entered in `docs/12-corrections.md`.

**Everything else on this disc is unopened at the moment this file is
written.** No member has been decompressed. No compression method has been
identified. `SOUNDW.LZ`, `TORREY_P.CRS`, `TITLE.SCR`, `GRAPHICS.LZ`,
`GOLFER_F.LZ`, `GOLFER_M.LZ` and `LINKS.CFG` have not been read past their
first bytes or at all. 261 of `GOLF.EXE`'s printable runs are unread.

### §A.3 — the calibration series, and which one is in use

The series handed over, as *predicted minus obtained* on open clauses:

```
+10.5  +7.5  +5.0  +2.0  -14.0  -2.0  +9.0  0.0  +19.75
```

The brief that delivered it **contradicts itself** about which of those nine
belongs to which population, and says so. It states both that the first five
were optical media and the sixth a file tree — putting −2.0 sixth and −14.0
fifth — and that the file-tree population has exactly one sample and that
sample is −14.0. Those cannot both be true. **This session did not resolve the
contradiction and does not use either partition.** It uses the nine-point
series as a whole, observes that it spans 33.75 points and is centred nowhere
useful, and **applies no offset**, which is the standing prescription.

The prescription it does apply is the specific one, because it is measured
rather than averaged: **clauses about the *contents* of an unopened container
score around 68 %, clauses about its *method* around 85 %.** The response is
not to shade the content clauses downward but to **write fewer of them**, and
to open a specimen first. One was opened; see §A.2.

Clause classification, counted by command in §D: every clause carries
`method` or `content`, and `inherited` or `open`.

---

## §B — inherited clauses

*These re-test what `_pre\` asserts. They are cheap and they are not the
session's work, but five sessions running have found an inherited figure
wrong, so they are worth testing rather than copying.*

**C01** `method` `inherited` — The image is 1,675 sectors of MODE1/2352 and
`tools/mode1.py` finds a correct 12-byte sync pattern and a mode byte of 1 on
**every one** of the 1,675 sectors, not merely on sector 0. *Predicted: 1.0*

**C02** `content` `inherited` — The sector census re-run from scratch
reproduces 1,675 present / 1,671 declared / 1,123 covered by files / 545
all-zero / 7 non-zero, and the seven are exactly 15, 16, 17, 18, 26, 34 and
1667. *Predicted: 1.0*

**C03** `content` `inherited` — Sector 1667 remains unexplained by any
structure on the disc: its 550 non-zero bytes beginning at offset 1,496 are
**not** the tail of any of the thirteen files, are not a directory record, and
do not decompress. I predict this session describes its shape and refuses to
name it. *Predicted: 1.0*

**C04** `method` `inherited` — `tools/mz.py` parses `GOLF.EXE` and reproduces
every header field in §A.1, including `nreloc` 268 and the identity
`(309−1) × 512 + 84 = 157,780`. *Predicted: 1.0*

**C05** `method` `inherited` — `tools/ne.py` refuses `GOLF.EXE` naming
`e_lfanew=380509147`, and `tools/pe.py` also refuses it. Both refusals are
correct and both are the session's free negative controls. *Predicted: 1.0*

**C06** `method` `inherited` — The `+24` variant flag is `01` on `TOPVIEW.LZ`
and `LIE.LZ` and `00` on the other seven, on **9 of 9 with no exception**, and
the split agrees with the ISO 9660 recorded dates on 9 of 9. Four of the nine
are already measured (§A.2); this clause predicts the remaining five.
*Predicted: 1.0*

**C07** `method` `inherited` — On all seven 1992-variant archives the
three-byte value at +25 equals the count byte at +10 multiplied by 17, exactly,
**7 of 7**. Two are already measured; this predicts the remaining five.
*Predicted: 1.0*

**C08** `content` `inherited` — Shannon entropy over the nine `MDmd` files,
computed by `tools/entropy.py` rather than ad hoc, falls in 7.69–7.96
bits/byte, and **every one of the nine exceeds 7.6**. I further predict that
the length bound that bit the previous session does **not** bite here: the
smallest archive is 5,444 bytes, so log₂ of its length is 12.4 and the ceiling
is 8.0 for all nine. *Predicted: 1.0*

**C09** `content` `inherited` — `TITLE.SCR` has entropy near 6.78 bits/byte,
opens `00 01 01 00 00 00 00 18`, and is **not** an `MDmd` archive.
*Predicted: 1.0*

**C10** `content` `inherited` — `GOLF.EXE` contains 270 printable runs of six
or more characters under the same definition the pre-briefing used, ±5.
*Predicted: 0.5*

**C11** `method` `inherited` — The root directory's System Use area contains
the byte sequences `TEXT` and `hscd`, and they are an Apple ISO 9660 extension
signature rather than anything Tandy or Meridian put there. *Predicted: 1.0*

**C12** `content` `inherited` — Sector 15 contains `MD20` and 15 non-zero
bytes in its first 37, and this session finds **no evidence connecting `MD20`
to `MDmd`**. The two-character overlap is a coincidence, and the dates prove
it: two `MDmd` archives predate the mastering by one and two years.
*Predicted: 1.0*

**C13** `method` `inherited` — The Maketat build string is `1(12) 31-Aug-92`
on a disc mastered 1992-10-23, and combined with the four builds already
recorded in the platform notes this makes it **more likely than not that the
Maketat build string does not track the mastering date**. *Predicted: 0.5*

**C14** `content` `inherited` — No Joliet supplementary volume descriptor
exists on this disc, and `iso9660.py --compare` reports one namespace only.
*Predicted: 1.0*

---

## §C — open clauses

*These are the session. Nobody has measured any of them.*

### The container

**C15** `method` `open` — The 17-byte directory record measured on `GOLF1.LZ`
generalises: **all seven 1992-variant archives** carry a directory at offset
122 of `count × 17` bytes, made of 13-byte NUL-padded names and 4-byte
little-endian offsets. *Predicted: 1.0*

**C16** `method` `open` — **The members tile the archive with residue 0.** On
each 1992-variant archive, the declared offsets are strictly increasing, the
first equals 122 + directory length, and the last member runs to the final byte
of the file with nothing left over. This is the check the format gives away
free, and it fires on 7 of 7. *Predicted: 1.0*

**C17** `method` `open` — The pre-1992 variant has **no offset directory at
all**. Its count byte is 0 because there is nothing to count: it stores a
single member, or it chains records inline, and in either case walking it
requires different code from the 1992 variant. *Predicted: 0.5*

**C18** `content` `open` — Across all nine archives the total member count is
**between 250 and 700**. (The seven measured count bytes in the pre-briefing
sum to 311 before the two old-variant files are resolved; this clause predicts
the census does not wildly exceed that.) *Predicted: 0.5*

**C19** `method` `open` — Member payloads are **compressed, not stored**. The
mean entropy of extracted member payloads exceeds 7.5 bits/byte on a majority
of members over 1,024 bytes. *Predicted: 1.0*

**C20** `method` `open` — The compression is an **LZ77-family byte-oriented
scheme with a sliding window and a bit-flag control byte** — the ordinary
1990-vintage shape — and **not** deflate, not LZW with a standard header, not
LZSS with an identifiable public header, and not RLE. I explicitly refuse to
predict that `.LZ` names the algorithm: `.LZ` is a filename, and this
repository has lost points twice on expanding an abbreviation.
*Predicted: 0.5*

**C21** `content` `open` — **A working decompressor is not reached this
session.** I predict the session identifies the container completely, extracts
every member as a compressed blob with a correct length, and stops there,
publishing a described shape and a stated refusal rather than a decoder.
*Predicted: 0.5*

**C22** `method` `open` — `MDmd` **nests**: at least one member of at least one
archive is itself an `MDmd` archive. (`SC.MLD` inside `SCORCARD.LZ` is already
measured, §A.2; this clause predicts nesting occurs in **at least one other
archive as well**.) *Predicted: 0.5*

**C23** `method` `open` — A reader given `LINKS.CFG` (14 bytes), `TITLE.SCR`
and `CONTROL.TAT` **refuses all three loudly**, and the 14-byte file is the
cheapest negative control on the disc. `--validate` runs before `--census`.
*Predicted: 1.0*

### What is inside

**C24** `content` `open` — `SOUNDW.LZ` contains **recorded digital audio** —
raw or lightly-coded PCM samples, not synthesiser scores, not FM instrument
tables, not MIDI. *Predicted: 0.5*

**C25** `content` `open` — The member names inside `SOUNDW.LZ` are **not**
recognisable words for sounds; they follow the `NNNN.BLK`-style pattern seen
in `LIE.LZ`'s `LIE00.BLK` rather than naming what they contain.
*Predicted: 0.5*

**C26** `content` `open` — `TORREY_P.CRS` contains **course geometry for
Torrey Pines**: per-hole data structures, and its member names contain hole
numbers or the strings `HOLE`, `GRN`, `TEE` or a digit pair. *Predicted: 0.5*

**C27** `content` `open` — `TITLE.SCR` **is a single raster image**, and its
opening `00 01 01 00 00 00 00 18` contains a dimension or a bit-depth field
rather than being a magic number. *Predicted: 0.5*

**C28** `content` `open` — **Something on this disc is made visible or audible
during this session and shown to the machine's owner.** At least one asset is
extracted to a form a human can open. *Predicted: 0.5*

### The program

**C29** `content` `open` — Reading all 270 printable runs of `GOLF.EXE`
produces **at least one string naming a member of an `MDmd` archive other than
`DROP2.BLK`**, tying the program's string table to the containers on more than
one witness. *Predicted: 1.0*

**C30** `content` `open` — `GOLF.EXE`'s strings contain **at least one
personal name** — a developer credit, a licensed golfer, or a course
architect. This repository declines to predict "no personal names": the last
five sessions that did were wrong twice, and a 1990 Access Software product is
exactly the vintage that ships a credit string. *Predicted: 0.5*

**C31** `content` `open` — `GOLF.EXE` contains an **overlay or segment-swap
scheme**: strings or structures naming loadable code parts, consistent with
`minalloc 0` / `maxalloc 1`. I predict this is *supported* by evidence beyond
the two header fields, which on their own are a reading and not a measurement.
*Predicted: 0.5*

**C32** `content` `open` — A **compiler or toolchain identification** for
`GOLF.EXE` is reachable from its strings, as it was for `SHI.EXE`, and it is
**not** Borland C++ — this is an Access Software title, not the Sherlock
vendor's. *Predicted: 0.5*

### The disc against the collection

**C33** `content` `open` — `crossall.py` against 112 directories, 79 with
`notes\`, `--skip` this repository, reports **zero shared whole-file hashes**.
*Predicted: 1.0*

**C34** `content` `open` — **`GOLFER_F.LZ` and `GOLFER_M.LZ` share at least
one member name**, and the shared names are a majority of the smaller
archive's directory. They are 37,240 and 40,400 bytes, built fifty seconds
apart, with identical count bytes. *Predicted: 1.0*

**C35** `content` `open` — But **no member payload is byte-identical between
any two archives on this disc**. Same names, different bytes: the two golfer
archives are the same structure holding different art. *Predicted: 0.5*

**C36** `content` `open` — `protscan.py` reports **zero** of its markers on
all thirteen files plus the image, its positive control fires, and it reports
opening **14** files. The marker count is **9** — counted from the tool, not
inherited from a brief that said eleven about a different tool.
*Predicted: 1.0*

**C37** `content` `open` — `hashall.py` covers 13 files plus the image = 14
objects, and **nothing needs excluding**; `--exclude-name` is not used, and
saying so after checking is the P.4 statement. *Predicted: 1.0*

**C38** `content` `open` — `LINKS.CFG`'s 14 bytes are **not saved user
state**: they are a fixed table shipped identically on the pressing, and P.4
holds. *Predicted: 1.0*

### The two decisions that are not measurements

**C39** `content` `open` — **The thesis figure lands below 50 %** and is
therefore the first VIS entry outside the nineties, and the session publishes
it over **more than one denominator**, following *Race the Clock*'s three.
*Predicted: 1.0*

**C40** `method` `open` — **The G1..G4 test cannot be run as written and the
session publishes an amendment rather than four ticks.** The amendment
relocates each question from NE resource structures to format-independent
evidence — a string table, a member-name directory, a segment map — and states
explicitly that a criterion which only works on one binary format is a
criterion about binary formats. *Predicted: 1.0*

**C41** `content` `open` — Under any honest reading of the amended test,
**`Links` qualifies as a game on at least three of the four criteria**, and
this is the first VIS disc in this collection to do so. *Predicted: 1.0*

**C42** `method` `open` — The session produces a **row for the platform
notes' `CONTROL.TAT` table** and a **title entry for the game list**, writes
both here as text, says where each goes, and **does not edit either
repository**. *Predicted: 1.0*

**C43** `content` `open` — The DOS-versus-Modular-Windows contradiction
**stays open**. The session moves the count to two DOS masters against one NE
title and describes the third specimen precisely, and explicitly refuses to
choose among the platform notes' four readings, because that is a fact about a
ROM shell. *Predicted: 1.0*

### Housekeeping

**C44** `content` `open` — `_work\` at the end of the session is **under 40
MB**, which would make it the smallest working set in seven sessions by more
than an order of magnitude. It is measured before it is deleted.
*Predicted: 0.5*

**C45** `method` `open` — `toolscan.py` over `.py`, `.md` and `.txt` with the
`0x00`, `0x01` and `0x1b` positive controls reports **0 findings across all
414 tools plus every document this session writes**, with all three controls
firing. Run at the halfway point and at the end. *Predicted: 1.0*

**C46** `content` `open` — The session publishes **fewer than 16 documents**,
against a ten-session run of 19, 20, 20, 20, 20, 17, 19, 19, 19, 19. A 3.9 MB
disc with thirteen files does not support nineteen honest chapters.
*Predicted: 1.0*

---

## §D — the arithmetic, done by command

Clause counts and predicted totals are computed by `tools/predcount.py` from
this file's own text. `checkscore.py`, inherited, counts verdict *tables* in a
scoring document and does not fit a predictions document; `predcount.py` was
written for this shape and fails loudly on a missing tag, a duplicated tag, a
missing figure, or a gap in the numbering.

    python tools/predcount.py

    clauses        : 46
      inherited    : 14
      open         : 32
      method       : 17
      content      : 29

      inherited method  : 7
      inherited content : 7
      open      method  : 10
      open      content : 22

    TWO TOTALS, NEVER SUMMED TOGETHER:
      inherited predicted : 13.00 of 14
      open      predicted : 24.50 of 32

    content share of the open clauses : 22 of 32 = 68.8 %

**On its first run the tool failed, and the failure was its own**: it
terminated the last clause at end-of-document, so C46 absorbed this section,
which contains the words `method` and `content`, and it reported a duplicate
tag. Fixed and re-run; the negative control (`--expect-open-total 99.0`) exits
non-zero.

**And the tool's closing line is a fair criticism of this document.** 68.8 % of
the open clauses are `content`, on a disc whose containers were opened once.
The prescription said write fewer of those. This document did not write
enough fewer. The clauses stand as written and the observation is recorded
here rather than fixed by editing, which would make the count meaningless.

### §D.1 — a population that changed after the predictions were written

**C33 predicts that `crossall.py` finds zero shared whole-file hashes against
the collection.** Minutes after this file was written and before any
cross-check was run, the owner of the machine created
`D:\Homebrew7\pc-linksthechallengeofgolf-doc\`, containing the MS-DOS release
of the same title: 32 files including `golf1.lz`, `lie.lz`, `topview.lz`,
`scorcard.lz`, `torrey_p.crs` and `links.cfg` — **the same names this disc
carries.**

The clause was written against a collection that did not contain that
directory. **It has not been edited and it will be scored as written.** The
collection denominator moved from 112 directories to 113 in the same interval,
and from 79 with a `notes\` to 80, the latter because this repository acquired
one. Both figures were measured twice, before and after, and both are
reported in `docs/09-collection.md`.

This is worth a line in the calibration record rather than a shrug: a
prediction about a population is only as stable as the population, and this one
moved under it inside an hour.

The results of the counting command are written into `docs/11-scoring.md` at
the end of the session, alongside the obtained score, and the delta is added to
the calibration series as a tenth point.

**Standing prescription, restated:** no global offset is applied. The specific
prescription — write fewer `content` clauses about unopened containers, and
open a specimen before writing any — was applied, and §A.2 records the
specimen that was opened.
