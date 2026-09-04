# 10 — against the collection: zero at the file level, and 197 of 197 one level down

*Measure: denominators published before the result — **113 directories in the
collection root, 80 with a `notes\`**. `crossall.py` finds **0 of 15** file
hashes and **0 of 309** member hashes in any other repository's published
hashes. Against the MS-DOS release of the same game, which arrived mid-session:
**2 of 7 same-named files byte-identical, and 197 of 197 same-named archive
members byte-identical.***

---

## The denominators, published first

    ls -d ../*/ | wc -l            ->  113
    ls -d ../*/notes/ | wc -l      ->   80

Both were measured twice, at the start of the session and again at the end,
and both moved in between:

* **112 → 113** because the owner of this machine created
  `pc-linksthechallengeofgolf-doc` during the session, for the MS-DOS release
  of this same game;
* **79 → 80** because this repository acquired the `notes\` it did not have
  when the first count was taken.

Both movements are accounted for and neither is a measurement error. The
consequence for a prediction written before them is recorded in
[14-corrections-and-scoring.md](14-corrections-and-scoring.md).

## The crossing, at two levels

    python tools/crossall.py notes/sha1-all.txt --collection .. \
        --skip vis-linksthechallengeofgolf-doc
    -> CROSSINGS: 0 of my 15 distinct hashes appear in another repository

    python tools/crossall.py notes/sha1-members.txt --collection .. \
        --skip vis-linksthechallengeofgolf-doc
    -> CROSSINGS: 0 of my 309 distinct hashes appear in another repository

**Zero at both levels**, which is what was predicted and is the boring correct
answer. The tool excludes eleven repositories whose published hash lists it can
see, and names them, so the zero is a zero over a stated population rather than
over a silence.

The last four objects in this series all produced their best cross-result one
level below the file, so the member-level list was built for the purpose: 320
members, 309 distinct, hashed after decompression. Against this collection it
finds nothing, because no other repository has ever opened an `MDmd`.

## The one cross-disc result that was already free

The platform notes carry a `CONTROL.TAT` comparison over five pressings. This
is the sixth, and it agrees on every axis they had recorded — and closes one
question they left open.

    python tools/controltat.py _work/iso/CONTROL.TAT
      md5 of the leading 84 bytes : ed9bfc904220e409f04c0772f1797ff7
      the platform notes' value   : ed9bfc904220e409f04c0772f1797ff7   MATCH

**[6 of 6] on the 84-byte identity block.** Six studios, six subjects, same
eighty-four bytes.

The notes' standing open question is this, in their words:

> **What is still unknown is what `fdiv` means.** Four characters, 2 of 2, and
> nobody has hexdumped byte `0xB0` of the other three pressings. **That is now
> the cheapest check on the platform.**

    bytes at 0x0B0 : 66 64 69 76 00 00 00 00   =  'fdiv'

**`fdiv` at `0x0B0` is now 3 of 3.** It still does not have a meaning, and this
disc supplies no evidence toward one; it supplies a third witness that the
field is constant across studios and across binary formats, which narrows what
it can be.

And the notes' length rule — *`CONTROL.TAT` = 463 + the length of its
NUL-terminated program list at `0x1A3`*, established on 5 of 5 — takes a sixth
row:

    program list at 0x1A3 : 'A:golf.exe'  = 10 chars, 11 with the NUL
    463 + 11 = 474        actual 474      MATCH

**[6 of 6].** The independent check agrees too: `Maketat` sits at `0x1AF` here
against `0x1AE` on *Race the Clock*, a difference of 1, and 474 − 473 = 1.

## The MS-DOS release, which arrived during the session

`D:\Homebrew7\pc-linksthechallengeofgolf-doc\` was created by the owner of this
machine while this session was running, holding the MS-DOS release of the same
title: 32 files, 1,584,769 bytes. **It is the next pipeline's object and this
repository does not document it.** It was used here as an instrument, because
it is the only other specimen of `MDmd` in existence as far as this collection
knows, and because it settles by measurement something this disc could
otherwise only infer from a date.

Seven names occur on both:

| name | VIS bytes | MS-DOS bytes | identical |
|---|---:|---:|---|
| `GOLF.EXE` | 157,780 | 60,950 | no |
| `GOLF1.LZ` | 10,088 | 22,706 | no |
| **`LIE.LZ`** | **11,262** | **11,262** | **YES** |
| `LINKS.CFG` | 14 | 14 | no |
| `SCORCARD.LZ` | 5,444 | 12,298 | no |
| **`TOPVIEW.LZ`** | **10,601** | **10,601** | **YES** |
| `TORREY_P.CRS` | 782,788 | 780,494 | no |

**`LIE.LZ` and `TOPVIEW.LZ` are byte-for-byte identical across the two
releases.** Those are precisely the two files whose ISO 9660 dates on this disc
read **1991-02-13** and **1990-10-09** — the two the pre-briefing identified,
from their dates alone, as the original product's files carried into the
pressing without being rebuilt.

**That inference is now a measurement**, and it is the strongest kind: not "the
date says 1990" but "the bytes are the same bytes".

## One level below the file, which is where the result is

Decompressing both sides and comparing member payloads:

| archive | VIS members | PC members | common names | byte-identical |
|---|---:|---:|---:|---:|
| `TORREY_P.CRS` | 184 | 186 | 184 | **184** |
| `LIE.LZ` | 6 | 6 | 6 | **6** |
| `TOPVIEW.LZ` | 3 | 3 | 3 | **3** |
| `GOLF1.LZ` | 9 | 13 | 4 | **4** |
| `SCORCARD.LZ` | 1 | 2 | 0 | — |
| **total** | | | **197** | **197 — 100.00 %** |

**Every single member the two releases share is the same bytes.** Not one was
recompressed, re-palettised or rebuilt. That is also, incidentally, a third
independent confirmation of the LZW implementation in
[05-the-codec.md](05-the-codec.md): the same decoder run over two different
1992 pressings produces 197 matching sha1s.

The differences are entirely in what each release adds and drops:

| | dropped by the VIS build | added by the VIS build |
|---|---|---|
| `TORREY_P.CRS` | `OBJECT.OFS`, `PATCH.OFS` | — |
| `GOLF1.LZ` | `OPTIONS.BLK`, `SETUP.BLK`, `PRCTPANL.BLK`, `MAINPANL.BLK`, `POSTSHOT.BLK`, `FLAG.BLK`, `CHGPOS.BLK`, `DROP.BLK`, `PLOTBALL.CEL` | `MAINPAN2.BLK`, `POSTSHT1/2` (in `GRAPHICS.LZ`), `DROP2.BLK`, `PLOTBALL.BLK`, `REDRAW.BLK`, `ROTATE.BLK` |
| `SCORCARD.LZ` | `SCORCARD.CEL`, `SCORCARD.MLD` | `SC.MLD` |

So the shape of the port is legible in one table. **The course, the lie tables
and the top view crossed unchanged. The interface was rebuilt.** `OPTIONS.BLK`
and `SETUP.BLK` went away, which is what happens to a configuration screen on a
console that has none — the MS-DOS release ships `setblast.exe`, `systype.exe`,
`xmm.exe`, `himem.sys` and `keybat.com` beside the game, and this disc ships
none of them.

Two more differences worth their own line.

**The sound was replaced entirely.** `sounds.lz` on MS-DOS is 92,406 bytes and
36 members; `SOUNDW.LZ` here is 891,212 bytes and 97 members. **Zero common
names, zero common payloads.** Ten times the size, and all of it new.

**`LINKS.CFG` is a shipped table, not saved state.** 12 of its 14 bytes are
identical between the two releases; byte 3 reads `0x01` here against `0x00`
there, and byte 8 reads `0x03` against `0x07`. That is the same record with two
platform fields set differently, and it settles P.4 by comparison rather than
by assertion — see [12-privacy.md](12-privacy.md).

**And the one-course design is not a VIS decision.** The MS-DOS release also
ships exactly one course, `torrey_p.crs`, with `practice.crx` beside it — a
practice area the VIS build does not carry at all. Additional courses were sold
separately for the PC. So the empty disc described in
[03-the-medium.md](03-the-medium.md) is the game's design travelling intact,
not a console-specific decision; what the VIS shipped is the same one-course
game plus a disc-swap prompt for courses that, as far as this collection knows,
never came.

---

## What this session proposes for two other repositories

**Produced here. Neither file is edited by this session.**

### For `vis-platformnotes-doc`, §2, the `CONTROL.TAT` table

Append this row:

```
Links: The Challenge of Golf   fdiv   Maketat - Version is 1(12) 31-Aug-92
```

and to the length table:

```
disc            program list at 0x1A3       len  base   pred actual
Links           A:golf.exe                   11   463    474    474  MATCH
                                                                 6 of 6
```

and, for the standing `fdiv` question: **`fdiv` at `0x0B0` is 3 of 3.**
Present on *Sherlock Holmes* (DOS MZ), *Race the Clock* (Win16 NE) and *Links*
(DOS MZ). It does not track the binary format.

### For `vis-platformnotes-doc`, §10, the baselines table

```
| Links: The Challenge of Golf | 1992 | Access Software | 1(12) 31-Aug-92 | 474
| `LINKS VIS Version 1.00 - Copyright Access Software Inc. 1992` | 13
| 3,430,400 | 0 | n/a — no import table | none; real-mode DOS
| yes, 988,479 B of 11,025 Hz PCM |
```

and to the two-column table beneath it:

```
| Links | `A:golf.exe` — one, real-mode DOS | 1 of 13 (`CONTROL.TAT`),
  474 B of 2,284,337 = 0.0207 % |
```

### For `vis-platformnotes-doc`, §1, the DOS question

The notes ask for a count and this disc supplies the third term:

> **The cheapest next step for whoever holds the other three pressings: does
> any of them contain an NE binary? That is one `find`.**

**The count is now two DOS masters against one Modular Windows title.**

| disc | program | format | vendor block names |
|---|---|---|---|
| *Sherlock Holmes* | `SHI.EXE`, 119,618 B | real-mode MZ | `A:MOUSE.COM`, `SHI.EXE` |
| *Race the Clock* | `RTC.EXE`, 136,192 B | **Win16 NE**, expects Windows 3.10 | `minwin A:` |
| ***Links*** | **`GOLF.EXE`, 157,780 B** | **real-mode MZ** | **`A:golf.exe`** |

And one new datum the notes will want, because it is about the machine rather
than the disc: **`GOLF.EXE` prints `LINKS requires at least 530K bytes of free
memory to operate properly` and opens `EMMXXXX0`.** A program that checks for
530 K of free conventional memory and probes for an expanded-memory manager was
written for a real-mode DOS machine, whatever the console does with it.

**This repository declines to choose among the notes' four readings.** Whether
the console runs DOS, whether the note is over-stated, whether these discs boot
as they stand, or whether they are DOS masters carrying a Tandy vendor block,
is a fact about a ROM shell. This disc moves the tally and describes the third
specimen precisely, and that is all 3.4 MB of golf can do.

### For `vis-gamelist-doc`

A title entry:

```
| Links: The Challenge of Golf | 1992 | Access Software | Tandy |
  G1 yes, G2 yes, G3 yes*, G4 yes | the first disc in this library that
  satisfies all four |
```

with the asterisk pointing at [09-the-game-test.md](09-the-game-test.md): **4 of
4 under the amendment proposed there, 3 of 4 on the original wording of G3**,
because the disc's penalties cost strokes rather than ending the round. The
amendment and both scores are that repository's to accept or reject.

### And one capability for the platform notes that came out of a picture

**The VIS supported content-disc swapping, and a shipped retail game used it.**
`CDDSP.MLD` renders *"The Course on this CD-ROM is:"* / *"Do you wish to use
this course?"*; `CDCHANGE.MLD` renders *"Please insert a new Course CD. Then
select Continue."* Both are named in `GOLF.EXE`'s string table and both were
rendered from the disc — see [06-the-pictures.md](06-the-pictures.md).

This is a claim about what the platform's software did, evidenced by that
software. It is **not** a claim about what the ROM shell supports, which this
repository has no way to measure.
