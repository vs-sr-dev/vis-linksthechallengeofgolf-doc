# 01 — the object: three and a half megabytes, of which eighty-one per cent was sealed

*Measure: one MODE1/2352 image of 3,939,600 bytes, 1,675 sectors, exact
divisor. Thirteen files, zero directories, 2,284,337 file bytes. Nine of the
thirteen are `MDmd` archives holding 320 members which expand to 3,069,433
bytes. Every figure in this chapter is reproduced in
[02-datasheet.md](02-datasheet.md) with the command that produced it.*

---

## What it is

*Links: The Challenge of Golf*, Access Software, published for the Tandy Video
Information System by Tandy Corporation. The disc's own vendor block says so
from inside:

    'LINKS VIS Version 1.00  -  Copyright Access Software Inc. 1992'

The volume was mastered **1992-10-23 at 20:42:28**, volume id `LINKSVIS`,
publisher `TANDY`, preparer `MERIDIAN_DATA_CD_PUBLISHER`. The program's own
build stamp, found in `GOLF.EXE`'s string table, reads `10-22-92 at 16:46` —
**four minutes before** the ISO 9660 recorded time on `GOLF.EXE` itself,
16:50:58 on the same day.

This is the third VIS disc this collection has documented and the first that is
a game in the ordinary sense of the word. It is also, by a wide margin, the
smallest object the series has opened: **the disc is 0.5030 % full**, its
3,430,400 bytes of user data against the 681,984,000 bytes a 74-minute Mode 1
CD-ROM holds. The pre-briefing said 0.55 %; that figure does not reproduce on
any standard capacity and is corrected in
[14-corrections-and-scoring.md](14-corrections-and-scoring.md).

## The four denominators, and which one a figure is on

Rule 3 of this pipeline says every figure names its denominator. This object
has four and they differ by a factor of 1.7, so the rule earns its keep.

| denominator | bytes | what it is |
|---|---:|---|
| raw image | **3,939,600** | 1,675 × 2,352, every physical byte including sync, header and ECC |
| user data | **3,430,400** | 1,675 × 2,048, the payload of every sector |
| file bytes | **2,284,337** | the thirteen files as the filesystem declares them |
| expanded content | **3,497,799** | every archive member decompressed, plus the four non-archive files |

**Coverage.** Of the raw image, 1,123 sectors (67.04 %) are covered by a file,
552 (32.96 %) by nothing. Of the 552, **545 are all-zero and 7 are not**. Of
the 2,284,337 file bytes, **1,855,971 — 81.2477 % — were inside nine `MDmd`
archives that nobody had opened** when this session began. All nine are open
now, all 320 members decode, and the container tiles with residue 0.

## The thirteen files

| path | bytes | extent | ISO 9660 date | what it turned out to be |
|---|---:|---:|---|---|
| `/CONTROL.TAT` | 474 | 35 | 1992-10-19 10:32:36 | Tandy vendor block, sixth pressing compared |
| `/GOLF.EXE` | 157,780 | 36 | 1992-10-22 16:50:58 | real-mode DOS MZ, 268 relocations |
| `/TITLE.SCR` | 270,098 | 114 | 1992-10-13 08:58:22 | the title screen: a 640 x 400 raster plus a 640 x 22 strip; byte encoding **not identified** |
| `/LINKS.CFG` | 14 | 246 | 1992-09-29 09:24:32 | shipped configuration, 12 of 14 bytes shared with the MS-DOS release |
| `/GRAPHICS.LZ` | 66,936 | 247 | 1992-10-16 07:46:40 | `MDmd`, 14 members: UI screens and help |
| `/SOUNDW.LZ` | 891,212 | 280 | 1992-07-01 16:56:36 | `MDmd`, **97 RIFF WAVE files** |
| `/GOLF1.LZ` | 10,088 | 716 | 1992-10-07 14:03:12 | `MDmd`, 9 members: panels and sprites |
| `/GOLFER_F.LZ` | 37,240 | 721 | 1992-09-01 13:55:58 | `MDmd`, 3 members: the female golfer |
| `/GOLFER_M.LZ` | 40,400 | 740 | 1992-09-01 13:56:48 | `MDmd`, 3 members: the male golfer |
| `/LIE.LZ` | 11,262 | 760 | **1991-02-13** 08:11:28 | `MDmd`, 6 members, **byte-identical to the MS-DOS release** |
| `/SCORCARD.LZ` | 5,444 | 766 | 1992-10-02 16:45:32 | `MDmd`, 1 member: the scorecard screen |
| `/TOPVIEW.LZ` | 10,601 | 769 | **1990-10-09** 14:45:28 | `MDmd`, 3 members, **byte-identical to the MS-DOS release** |
| `/TORREY_P.CRS` | 782,788 | 775 | 1992-08-14 15:24:26 | `MDmd`, 184 members: Torrey Pines South |

**Zero directories.** The path table is ten bytes long, which is the flattest
tree in this collection.

## What the object preserves

**The 1990 product, intact and identifiable.** Two files carry ISO 9660 dates
that predate the VIS port by one and two years, and the pre-briefing inferred
from those dates that they were the MS-DOS original's files carried across
without rebuilding. **That inference is now a measurement**: `LIE.LZ` and
`TOPVIEW.LZ` are byte-for-byte identical to the files of the same name in the
MS-DOS release. So is every one of the 184 members of `TORREY_P.CRS`. Across
the five archives the two releases share, **197 of 197 common member names are
byte-identical** — see [10-against-the-collection.md](10-against-the-collection.md).

**A complete asset set for one golf course.** 135 terrain patches, 34 objects,
nine panorama strips, a top-down map, a photograph of the course with its own
caption, a palette, and a course header.

**Eighty-nine seconds of recorded human speech**, in 97 RIFF WAVE files, 96 of
them 11,025 Hz 8-bit mono. Nineteen are stored uncompressed and play as they
sit on the disc.

**A string table that documents the disc from inside.** `GOLF.EXE` names
**263 of the 318 distinct member names — 82.7 %** — either verbatim or through
one of three literal templates it also carries (`PATCHxx.PAT`, `OBJxx.BLK`,
`BCKSND0.WAV`). The program explains the containers.

## What the object loses

**Everything the console could not offer.** The MS-DOS release's `GOLF1.LZ`
carries `OPTIONS.BLK`, `SETUP.BLK` and `PRCTPANL.BLK`; the VIS build drops all
three and adds five of its own. There is no `himem.sys`, no `xmm.exe`, no
`setblast.exe`, no `systype.exe`, no `.plr` player files and no `manual.txt` —
the MS-DOS release ships all of those and this disc ships none of them.

**The other courses.** The disc holds one course. `GOLF.EXE` names
`CDCHANGE.MLD` and the screen it draws reads *"Please insert a new Course CD.
Then select Continue."* — so the architecture for more was shipped and the
content was not.

**Any moving picture at all.** The two prior VIS discs in this collection are
96.4344 % and 95.8047 % video. **This disc contains no video**, and that is
what makes its thesis figure the first outside the nineties. See
[11-the-thesis.md](11-the-thesis.md).

**A third of itself.** 545 sectors of user area are all zero and belong to no
file. Somebody chose to master 3.4 MB onto a 650 MB medium, and a third of
what they wrote is nothing.
