# 02 — datasheet: every figure with the command that makes it again

*Measure: this chapter exists so that no number anywhere else in this
repository has to be believed. Each row carries the command. Paths are given
relative to the repository root; the image and the extracted files live in
`_work\`, which is not published.*

Reproducing the working tree from the disc:

    python tools/iso9660.py "Links - The Challenge of Golf (USA).bin" --extract _work/iso
    -> extracted 13 files, 2284337 bytes, to _work/iso

---

## The medium

| figure | value | command |
|---|---:|---|
| image length | 3,939,600 B | `ls -l` |
| sector size | 2,352 | `.cue` says `MODE1/2352` |
| sectors | 1,675 exact | 3,939,600 / 2,352 |
| user data | 3,430,400 B | 1,675 × 2,048 |
| sectors validated as Mode 1 | 1,675 / 1,675 | `python tools/mode1.py IMG --census` |
| EDC/ECC layout confirmed | 32 / 32 | `python tools/mode1.py IMG --validate` |
| P parity mismatches | 1 | `--census` |
| Q parity mismatches | 1 | `--census` |
| sectors with a non-zero reserved field | 1 (LBA 1667) | `--census` |
| declared volume space | 1,671 blocks | `python tools/iso9660.py IMG --vd` |
| run-out | 4 sectors | 1,675 − 1,671 |

## The filesystem

| figure | value | command |
|---|---:|---|
| files | 13 | `python tools/iso9660.py IMG --tree` |
| directories | 0 | same |
| file bytes | 2,284,337 | same |
| file sectors | 1,123 | same |
| path table size | 10 B | `--vd` |
| Joliet supplementary descriptor | none | `--compare` |

## The sector accounting, re-derived from scratch

    python - <<'EOF'   # reproduced in full in 03-the-medium.md
    ... walk the 13 extents, mark covered sectors, test each uncovered one
    EOF

| figure | value |
|---|---:|
| sectors in image | 1,675 |
| covered by the 13 files | 1,123 |
| covered by nothing | 552 |
| of those, all-zero user area | **545** |
| of those, not all-zero | **7** — LBA 15, 16, 17, 18, 26, 34, 1667 |
| zero-user sectors *inside* a file extent | 26 |
| total zero-user sectors | **571** |

The last row reconciles this accounting with `mode1.py --census`, which reports
571 and counts every sector rather than only the uncovered ones. 545 + 26 = 571.
Two tools, two definitions, one agreement.

## The container, `MDmd`

    python tools/mdmd.py _work/iso/LINKS.CFG _work/iso/TITLE.SCR _work/iso/CONTROL.TAT _work/iso/GOLF.EXE --validate
    -> all four REFUSED; exit 1
    python tools/mdmd.py _work/iso/*.LZ _work/iso/TORREY_P.CRS --validate
    -> all nine OK, residue 0; exit 0
    python tools/mdmd.py _work/iso/*.LZ _work/iso/TORREY_P.CRS --census
    python tools/mdmd.py _work/iso/*.LZ _work/iso/TORREY_P.CRS --verify

| file | bytes | nodes | dirs | members | raw | stored | ratio |
|---|---:|---:|---:|---:|---:|---:|---:|
| `GOLF1.LZ` | 10,088 | 10 | 1 | 9 | 58,416 | 8,715 | 0.1492 |
| `GOLFER_F.LZ` | 37,240 | 4 | 1 | 3 | 65,216 | 36,701 | 0.5628 |
| `GOLFER_M.LZ` | 40,400 | 4 | 1 | 3 | 69,184 | 39,861 | 0.5762 |
| `GRAPHICS.LZ` | 66,936 | 15 | 1 | 14 | 521,635 | 64,868 | 0.1244 |
| `LIE.LZ` | 11,262 | 6 | 0 | 6 | 24,000 | 10,530 | 0.4387 |
| `SCORCARD.LZ` | 5,444 | 2 | 1 | 1 | 64,791 | 5,183 | 0.0800 |
| `SOUNDW.LZ` | 891,212 | 98 | 1 | 97 | 992,747 | 877,607 | 0.8840 |
| `TOPVIEW.LZ` | 10,601 | 3 | 0 | 3 | 39,968 | 10,235 | 0.2561 |
| `TORREY_P.CRS` | 782,788 | 185 | 1 | 184 | 1,233,476 | 757,090 | 0.6138 |
| **total** | **1,855,971** | **327** | **7** | **320** | **3,069,433** | **1,810,790** | **0.5899** |

**Members decoding to their declared length: 320 of 320.**

Member name extensions, 320 members: `PAT` 135, `WAV` 97, `BLK` 76, `MLD` 8,
none 7, `COL` 2, `DAT` 1, `HDR` 1.

Build paths carried in node headers, 7 distinct: `C:\LINKS\TEMP\` ×188,
`WAV\` ×79, `C:\LINKS\SOUNDS\` ×6, `C:\LINKS\TOPVIEW\` ×2, `..\ANI\` ×1,
`C:\WIN31\` ×1, `C:\LINKS\GRAPHIC\` ×1.

## Entropy, with the length bound stated

    python - # Shannon entropy per file, with cap = min(8, log2(n))

| file | bytes | entropy | cap | fraction of cap |
|---|---:|---:|---:|---:|
| `CONTROL.TAT` | 474 | 4.3374 | 8.0000 | 0.5422 |
| `GOLF.EXE` | 157,780 | 4.4799 | 8.0000 | 0.5600 |
| `GOLF1.LZ` | 10,088 | 7.7058 | 8.0000 | 0.9632 |
| `GOLFER_F.LZ` | 37,240 | 7.8553 | 8.0000 | 0.9819 |
| `GOLFER_M.LZ` | 40,400 | 7.8678 | 8.0000 | 0.9835 |
| `GRAPHICS.LZ` | 66,936 | 7.9557 | 8.0000 | 0.9945 |
| `LIE.LZ` | 11,262 | 7.8343 | 8.0000 | 0.9793 |
| `LINKS.CFG` | 14 | 2.8963 | **3.8074** | 0.7607 |
| `SCORCARD.LZ` | 5,444 | 7.8722 | 8.0000 | 0.9840 |
| `SOUNDW.LZ` | 891,212 | 7.7451 | 8.0000 | 0.9681 |
| `TITLE.SCR` | 270,098 | 6.7798 | 8.0000 | 0.8475 |
| `TOPVIEW.LZ` | 10,601 | 7.9016 | 8.0000 | 0.9877 |
| `TORREY_P.CRS` | 782,788 | 7.6929 | 8.0000 | 0.9616 |

**The bound bites on exactly one file.** `LINKS.CFG` is 14 bytes, so it cannot
exceed log₂(14) = 3.8074 bits/byte however it is written. On the nine archives
the smallest is 5,444 bytes and the cap is a flat 8.0, so the range
7.6929–7.9557 means what it appears to mean. Saying which files a range covers
is the whole of the lesson this row exists for.

## The executable

    python tools/mz.py _work/iso/GOLF.EXE
    python tools/ne.py _work/iso/GOLF.EXE    # refuses, correctly
    python tools/pe.py _work/iso/GOLF.EXE    # refuses, correctly

| field | value |
|---|---:|
| `e_cp` / `e_cblp` | 309 / 84 |
| (309 − 1) × 512 + 84 | **157,780 = the file length, residue 0** |
| `e_cparhdr` | 96 paragraphs = 1,536 B |
| `e_crlc` | 268 relocations at 0x1C, 1,072 B |
| relocations landing inside the load image | 268 / 268 |
| **distinct relocation segments** | **3** |
| header slack after the table | 436 B, all zero |
| `e_minalloc` / `e_maxalloc` | 0 / 1 |
| `cs:ip` / `ss:sp` | 16AE:553E / 165A:0540 |
| `e_lfanew` at 0x3C | 380,509,147 — not a pointer; the relocation table runs through 0x3C |
| `NE` / `LE` / `LX` / `PE\0\0` occurrences | 5 / 8 / 0 / 0, none a header |
| printable runs of 6+ | **270** |

## The vendor block

    python tools/controltat.py _work/iso/CONTROL.TAT

| figure | value |
|---|---|
| length | 474 B |
| md5 of leading 84 B | `ed9bfc904220e409f04c0772f1797ff7` — **MATCH** against the platform notes |
| sha1 whole file | `056c25eb55c4aead72dfc77438b3c442be994b3a` |
| title field, 0x54 | `LINKS VIS Version 1.00  -  Copyright Access Software Inc. 1992` |
| `fdiv` at 0x0B0 | **present** |
| program list, 0x1A3 | `A:golf.exe` — one name |
| Maketat build, 0x1AF | `Maketat - Version is 1(12) 31-Aug-92` |
| zero bytes | 167 of 474 (35.23 %) |

## Hashes and the negative

    python tools/hashall.py _work/hashroot > notes/sha1-all.txt
    python tools/protscan.py _work/hashroot --all-files
    python tools/protscan.py _work/members --all-files

| figure | value |
|---|---:|
| objects hashed | 15 (13 files + image + cue) |
| distinct sha1 | 15 |
| unreadable | 0 |
| excluded | **0** — `--exclude-name` was not used |
| `--exclude-name NOSUCHFILE.XYZ` | reports `ERROR: matched no file` |
| protscan markers in the tool | **11**, counted from `MARKERS` at lines 23–33 |
| marker hits, 15 files, 6,224,038 B | **0 on all eleven** |
| marker hits, 320 members, 3,069,433 B | **0 on all eleven** |
| positive control (four zero bytes) | fires: 13 files, then 94 members |

## The audio

    python tools/wavcheck.py _work/members/SOUNDW.LZ

| figure | value |
|---|---:|
| files matching `.wav` | 97 |
| parsed as RIFF/WAVE | **97** |
| format tags | 1 × 97 (PCM) |
| 11,025 Hz mono 8-bit | 96 |
| 22,050 Hz mono 8-bit | 1 |
| duration, route A and route B | 89.133968254 s, agreeing to 0.000000000 s |
| `nAvgBytesPerSec ≠ rate × frame` | 0 files |
| chunk walk not accounting for every byte | 1 (`BRIAN.WAV`, walks 2,082 of 2,081) |
| PCM data-chunk bytes | 988,479 |
| distinct payloads | **91 of 97** |
| distinct PCM bytes | 937,471 |

## Tools

**418 Python files in `tools/`** — the 414 inherited from
`android-dissidiaduellum-doc` plus four written here.

    python tools/toolscan.py tools .py   ->  418 scanned, 0 findings
    python tools/toolscan.py docs .md    ->  0 findings
    python tools/toolscan.py notes .txt  ->  0 findings
    all three positive controls (0x00, 0x01, 0x1B) fire on every run.

**Written this session, and why each had to be:**

| tool | why |
|---|---|
| `mdmd.py` | there was no reader for the container holding 81.2477 % of the bytes |
| `mldpng.py` | there was no reader for the `GIFM` pictures inside it |
| `palscore.py` | a palette had four plausible readings and eyeballing chose wrong |
| `predcount.py` | rule 1 wants the clause count and score total by command, and `checkscore.py` counts a different document shape |

**Ran and applied, unmodified:** `iso9660.py`, `controltat.py`, `mz.py`,
`ne.py`, `pe.py`, `mode1.py`, `hashall.py`, `protscan.py`, `wavcheck.py`,
`crossall.py`, `toolscan.py`, `magic_sweep.py`.

**Looked at and did not apply.** `unityarc.py`, `unityasset.py`,
`il2cppmeta.py`, `apksigblock.py`, `apkinfo.py`, `apkcert.py`, `apkcensus.py`,
`apkbudget.py`, `bundlebudget.py`, `mp4probe.py`, `texdump.py`, `elfinfo.py`,
`unityfs.py`, `fsb5.py` — fifteen tools written for or extended on an Android
object. There is no APK, no Unity, no ELF and no ISO Base Media file here.
`avi.py`, `avirle.py`, `avicheck.py`, `tiles.py` — five sessions of AVI
machinery, and **this disc has no video**. `aif.py`, `isf.py`, `mpac.py`,
`slz.py`, `aska*.py`, `tales_block.py` — Bandai Namco *Tales* formats, which do
not transfer.

`magic_sweep.py` was run on `TITLE.SCR` and is the clean negative: its corpus
is *Tales* container tags and it reports zero on all of them, which is what a
tool honestly saying "not mine" looks like.

`blockrepeat.py` was the one Android tool the pre-briefing flagged as worth a
thought, on the grounds that it counts repeated fixed-width blocks and would
say something about the compressed streams. It was **not needed**: the streams
were decoded outright, and repeated content was measured directly instead —
11 of 320 member payloads are duplicates, all of them inside a single archive.
