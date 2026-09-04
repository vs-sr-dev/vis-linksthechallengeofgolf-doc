# Links: The Challenge of Golf (Tandy VIS, 1992) — a documentation pass

A measured description of the Tandy Video Information System pressing of
*Links: The Challenge of Golf* (Access Software, 1992): a 3.9 MB CD-ROM image
whose thirteen files include nine archives in an undocumented container that
nobody in this collection had ever opened.

**No bytes of the product are published here.** Hashes, formats, measurements
and four new tools; no assets, no image, no executable.

---

## The short card

| | |
|---|---|
| object | `Links - The Challenge of Golf (USA).bin` + `.cue`, MODE1/2352 |
| raw image | **3,939,600 B** = 1,675 sectors × 2,352, exact |
| user data | 3,430,400 B; **declared 1,671 blocks**, residue 4 of run-out |
| volume | `LINKSVIS`, publisher `TANDY`, preparer `MERIDIAN_DATA_CD_PUBLISHER` |
| mastered | **1992-10-23 20:42:28**; the program was linked at 16:46 the day before |
| filesystem | **13 files, 0 directories**, 2,284,337 file bytes, no Joliet |
| how full | **0.5030 %** of a 74-minute CD-ROM; **545 sectors are all zero** |
| the container | **`MDmd`** — 9 files, 1,855,971 B, **81.2477 %** of the file bytes |
| opened | **327 nodes, 320 members, residue 0 on 9 of 9**, expanding to 3,069,433 B |
| the codec | **variable-width LZW, LSB-first, clear 256, ceiling 13 bits** — **320 of 320 members decode to their declared length** |
| the pictures | **`GIFM` is a GIF** whose image data is raw indices, because the archive above it already applies GIF's LZW |
| the sound | **97 RIFF WAVE**, 96 at 11,025 Hz, **89.134 s** of a golf commentator; 19 stored uncompressed |
| the program | `GOLF.EXE`, 157,780 B, **real-mode DOS MZ**, `ne.py` and `pe.py` both refuse it |
| the bridge | `GOLF.EXE` names **263 of 318 member names — 82.7 %** — verbatim or by template |
| against the MS-DOS release | **197 of 197 shared archive members are byte-identical** |
| thesis | **43.2720 %** of file bytes is recorded PCM; 41.0391 % distinct; 25.0908 % of the raw image |
| the game test | **4 of 4** on the amendment proposed here; 3 of 4 on the original wording |
| protection | **0 hits on 11 markers**, over 15 files and again over 320 members |
| score | inherited **13.00 / 13.00**; open **19.25 / 24.50** |

## What this session found that was not known

* **The container is a recursive node tree**, not "an archive with a
  directory": a 122-byte header at every depth, and a tiling check whose two
  numbers come from two places and agree 320 times.
* **The codec is LZW and the file extension had nothing to do with finding
  it.** Nineteen members are stored uncompressed and every one is a `RIFF`
  header — a known-plaintext attack the format hands over for free.
* **`GIFM` is a GIF with the compression hoisted one level up**, into the
  archive, so a picture and a `.WAV` get the same treatment and a picture is
  never compressed twice.
* **The two files dated 1990 and 1991 really are the MS-DOS original's**, not
  by inference from a date but because they are byte-identical to it — as is
  every one of the 197 archive members the two releases share.
* **The VIS port rebuilt the interface and replaced the sound entirely**, and
  kept the course, the lie tables and the top view unchanged.
* **One member of one archive was picked up from `C:\WIN31\`**, and it is the
  Windows 3.1 chime — confirmed three independent ways.
* **The disc's own interface asks the player to swap in another Course CD**, on
  a platter with room for 866 more courses.
* **`TITLE.SCR` is a 640 x 400 picture plus a separate 640 x 22 strip** reading
  `Press BUTTON (A) to Continue`, and it carries `(C) 1992 ACCESS SOFTWARE INC`
  drawn into the artwork — the only year anywhere on the disc outside a date
  field. Its bytes are **not palette indices**: index 0 forms 45 pure blocks
  whose true colours run from orange through black to blue and magenta, and
  every byte in an even column has bit 7 clear, 128,000 of 128,000.
* **Sector 1667 is the only sector in the image whose reserved field is
  non-zero and whose P and Q parity both fail**, and it is still unexplained.

## Chapters

| | |
|---|---|
| [00 — predictions](docs/00-predictions.md) | written before the disc was opened, 46 clauses, wrong ones left standing |
| [01 — the object](docs/01-object.md) | what it is, four denominators, what it keeps and loses |
| [02 — datasheet](docs/02-datasheet.md) | every figure with the command that makes it again |
| [03 — the medium](docs/03-the-medium.md) | a chapter about emptiness, and one sector nobody can explain |
| [04 — the container](docs/04-the-container.md) | `MDmd`: a tree of identical nodes, tiling with residue 0 |
| [05 — the codec](docs/05-the-codec.md) | LZW, found by known plaintext, 320 of 320 |
| [06 — the pictures](docs/06-the-pictures.md) | `GIFM` is a GIF, and the colour table is ten bytes from where it looks |
| [07 — the sound](docs/07-the-sound.md) | 89 seconds of a commentator, and one chime from `C:\WIN31\` |
| [08 — the executable](docs/08-the-executable.md) | a DOS program that names 82.7 % of the disc's members |
| [09 — the game test](docs/09-the-game-test.md) | a criterion that only works on one binary format is a criterion about binary formats |
| [10 — against the collection](docs/10-against-the-collection.md) | zero at the file level, 197 of 197 one level down |
| [11 — the thesis](docs/11-the-thesis.md) | the first VIS entry outside the nineties |
| [12 — privacy](docs/12-privacy.md) | three corporate names, one first name in a filename, no user state |
| [13 — leftovers](docs/13-leftovers.md) | what is described and not explained |
| [14 — corrections and the score](docs/14-corrections-and-scoring.md) | fourteen corrections, eight of them mine |

## Tools

Four written here, in `tools/`:

* **`mdmd.py`** — reader for the container. `--validate` before `--census`,
  `--verify` decodes every member against its declared length, and it refuses
  four negative controls that ship on the disc.
* **`mldpng.py`** — renders the `GIFM` pictures to PNG using only `zlib`.
* **`palscore.py`** — decides which reading of a palette is right, **against
  controls**. The method is `vis-sherlockholmes-doc`'s and the citation is the
  point.
* **`predcount.py`** — counts prediction clauses and sums the two score columns
  by command, and fails loudly on a missing tag or a gap in the numbering.

414 more are inherited. `toolscan.py` reports **0 findings over 418 `.py`
files** plus every `.md` and `.txt` this session wrote, with all three positive
controls firing.

## Reproducing

    python tools/iso9660.py "Links - The Challenge of Golf (USA).bin" --extract _work/iso
    python tools/mdmd.py _work/iso/LINKS.CFG --validate          # must refuse
    python tools/mdmd.py _work/iso/*.LZ _work/iso/TORREY_P.CRS --validate
    python tools/mdmd.py _work/iso/*.LZ _work/iso/TORREY_P.CRS --verify
    python tools/mdmd.py _work/iso/GRAPHICS.LZ --extract _work/members/GRAPHICS.LZ
    python tools/mldpng.py --dir _work/members/GRAPHICS.LZ _work/png

The image, the cue sheet and everything derived from them stay in `_work\` and
are not published.

## Related

* [`vis-platformnotes-doc`](../vis-platformnotes-doc) — the platform checklist.
  This disc adds a sixth `CONTROL.TAT` row and makes `fdiv` at `0x0B0` 3 of 3.
* [`vis-gamelist-doc`](../vis-gamelist-doc) — the index and the G1–G4 test.
  [Chapter 09](docs/09-the-game-test.md) proposes an amendment to it.
* [`vis-sherlockholmes-doc`](../vis-sherlockholmes-doc) — the first VIS disc,
  and the source of the palette method used here.
* [`vis-racetheclock-doc`](../vis-racetheclock-doc) — the second, and the source
  of the multi-denominator thesis figure.
* `pc-linksthechallengeofgolf-doc` — the MS-DOS release of the same title, used
  here as a measuring instrument and documented elsewhere.
