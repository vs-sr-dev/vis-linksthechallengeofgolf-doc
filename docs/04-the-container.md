# 04 — `MDmd`: not an archive with a directory, a tree of identical nodes

*Measure: nine files, 1,855,971 bytes, 81.2477 % of the disc's file bytes,
opened. **327 nodes, 7 directories, 320 members, residue 0 on 9 of 9.** The
122-byte header is the same at every depth. The tiling check is not free: a
child's size comes from the child's own header and its position from the
parent's directory, so the two numbers come from two places and have to agree.
They agree 320 times.*

---

## What it actually is

The pre-briefing described `MDmd` as "an archive with a directory" and derived
a 17-byte directory stride from arithmetic, flagging the stride as **an
inference and not a measurement**. The inference was right and the description
was not.

`MDmd` is a **recursive node format**. Every node, at every depth, begins with
the same 122-byte header. A file is a *chain* of one or more such nodes laid
end to end.

    offset  size  meaning                                    witnesses
    ------  ----  -----------------------------------------  ---------
       0      4   magic 'MDmd'                               327 of 327 nodes
       4      4   constant 0a 01 7a 00                       327 of 327
       8      2   zero                                       327 of 327
      10      1   child count                                327 of 327
      11     13   zero                                       327 of 327
      24      1   compression flag: 0 stored, 1 packed       327 of 327
      25      3   u24 LE: raw (decompressed) length          327 of 327
      28      1   zero                                       327 of 327
      29      3   u24 LE: stored (on-disc) length            327 of 327
      32      9   not identified; not constant
      41      1   length of the name, 0..12
      42     12   name, 8.3, space-padded
      54      1   length of the build path, 0..67
      55     67   build path, space-padded to offset 122
     122      -   payload

    count > 0 : the payload is a DIRECTORY of count x 17 bytes -- 13 bytes of
                NUL-padded name, then a u32 LE ABSOLUTE file offset.
    count = 0 : the payload is a FILE of `stored` bytes.

**So the 17-byte stride is measured now**, and it is 13 + 4 rather than 12 + 5.
In `GOLF1.LZ` the directory at offset 122 reads

    WNDARROW.BLK -> 275     ADRESIND.BLK -> 739     BALLMRKR.BLK ->   987
    SPLASH.BLK   -> 1207    MAINPAN2.BLK -> 1709    DROP2.BLK    ->  5609
    PLOTBALL.BLK -> 9272    ROTATE.BLK   -> 9408    REDRAW.BLK   ->  9733

with count 9, directory length 9 × 17 = 153, and **122 + 153 = 275**, which is
the first member's declared offset.

## The byte at +24 is a compression flag, and reading it as anything else costs you

The pre-briefing found that the byte at +24 is `01` on exactly the two files
whose ISO 9660 dates predate the VIS port and `00` on the seven dated 1992 —
**9 of 9, no exception** — and read it as a generation marker: two eras of the
same archiver, the older one arriving with the MS-DOS original.

**The correlation is real and the explanation is wrong.** Every one of
`GOLF1.LZ`'s nine children carries `01`, and they were written the same day as
their parent. The byte says *this payload is compressed*. Directories are never
compressed, so a node with children always carries `0`; the two pre-1992 files
are flat chains of compressed members with no index node on top, and the seven
1992 files each open with an index. The date correlation is a by-product of a
structural difference, not a version stamp.

**The first version of this reader got this wrong too, in the opposite
direction**: it asserted that `+24 = 0` implied a directory. It refused
`SOUNDW.LZ` at its member `CLAPLOUD.WAV` — no children, flag `0`, raw ==
stored == 38,828 — and the refusal was the reader working. `CLAPLOUD.WAV` is a
**file stored verbatim**, and nineteen of the disc's ninety-seven WAVs are.

The invariant that makes the flag a measurement rather than a reading is
enforced in `parse_header`:

    flag == 0  =>  raw == stored          (327 of 327)
    count > 0  =>  flag == 0 and raw == stored == count * 17   (7 of 7)

## The check the format gives away, and why it is not free

A container that tiles by construction proves nothing. This one does not tile
by construction:

* a child's **size** is `122 + stored`, read from the **child's own header**;
* a child's **position** is a u32 read from the **parent's directory**.

`--validate` requires, for every parent:

    first child offset      ==  parent + 122 + count * 17
    child[i] + 122 + stored ==  child[i+1] offset
    last child ends exactly at the end of the parent
    a top-level chain covers the file exactly

    python tools/mdmd.py _work/iso/*.LZ _work/iso/TORREY_P.CRS --validate

    GOLF1.LZ         OK  10088 bytes, 1 top-level node(s), 10 nodes, 1 directory, 9 members, residue 0
    GOLFER_F.LZ      OK  37240 bytes, 1 top-level node(s), 4 nodes, 1 directory, 3 members, residue 0
    GOLFER_M.LZ      OK  40400 bytes, 1 top-level node(s), 4 nodes, 1 directory, 3 members, residue 0
    GRAPHICS.LZ      OK  66936 bytes, 1 top-level node(s), 15 nodes, 1 directory, 14 members, residue 0
    LIE.LZ           OK  11262 bytes, 6 top-level node(s), 6 nodes, 0 directories, 6 members, residue 0
    SCORCARD.LZ      OK  5444 bytes, 1 top-level node(s), 2 nodes, 1 directory, 1 member, residue 0
    SOUNDW.LZ        OK  891212 bytes, 1 top-level node(s), 98 nodes, 1 directory, 97 members, residue 0
    TOPVIEW.LZ       OK  10601 bytes, 3 top-level node(s), 3 nodes, 0 directories, 3 members, residue 0
    TORREY_P.CRS     OK  782788 bytes, 1 top-level node(s), 185 nodes, 1 directory, 184 members, residue 0

**Nine of nine, residue 0.** The prettiest instance is `TOPVIEW.LZ`, which has
no directory at all and whose three chained nodes cover it exactly:

    (122 + 703) + (122 + 9001) + (122 + 531)  =  825 + 9123 + 653  =  10,601

## The negative controls, run before the census

Rule 4: validate before census, on a specimen that has to fail. The disc ships
four free ones.

    python tools/mdmd.py _work/iso/LINKS.CFG _work/iso/TITLE.SCR \
                         _work/iso/CONTROL.TAT _work/iso/GOLF.EXE --validate

    LINKS.CFG        TILING FAILED  14 trailing bytes at 0, too few for a header
    TITLE.SCR        REFUSED  no MDmd magic at 0 (found b'\x00\x01\x01\x00')
    CONTROL.TAT      REFUSED  no MDmd magic at 0 (found b'Copy')
    GOLF.EXE         REFUSED  no MDmd magic at 0 (found b'MZT\x00')
    exit 1

The 14-byte `LINKS.CFG` is the cheapest of them and it fails on a different
rule from the other three, which is better than failing on the same one.

## What is inside

| extension | members | what they are |
|---|---:|---|
| `.PAT` | 135 | terrain patches, all in `TORREY_P.CRS`, named `PATCHnn.PAT` |
| `.WAV` | 97 | RIFF WAVE, all in `SOUNDW.LZ` — see [07-the-sound.md](07-the-sound.md) |
| `.BLK` | 76 | sprites, panels and panoramas |
| `.MLD` | 8 | full-screen `GIFM` pictures — see [06-the-pictures.md](06-the-pictures.md) |
| none | 7 | the `~INDEX~` directory nodes |
| `.COL` | 2 | 768-byte palettes, 6-bit VGA |
| `.DAT` | 1 | `TOPVIEW.DAT`, 224 bytes, stored verbatim |
| `.HDR` | 1 | `COURSE.HDR`, 4,030 bytes |

**Expansion: 1,810,790 stored bytes decompress to 3,069,433, a factor of
1.6951.**

## The build paths, which turn out to be truthful

Seven distinct paths appear in the 122-byte headers, and they are the
directories on somebody's development machine in 1990–1992:

| path | nodes |
|---|---:|
| `C:\LINKS\TEMP\` | 188 |
| `WAV\` | 79 |
| `C:\LINKS\SOUNDS\` | 6 |
| `C:\LINKS\TOPVIEW\` | 2 |
| `..\ANI\` | 1 |
| **`C:\WIN31\`** | **1** |
| `C:\LINKS\GRAPHIC\` | 1 |

**The single `C:\WIN31\` node is `DING.WAV`, and `DING.WAV` is the standard
Microsoft Windows 3.1 system chime.** The owner of this machine identified it
by ear without being told where its header said it came from. One member out of
320 claims to have been picked up from a Windows directory, and it is the one
member that is a Windows system sound. **The path field is not decoration.**

That is also a small, specific fact about how this port was built: somebody
working on a DOS golf game for a Modular Windows console reached into
`C:\WIN31\` for a notification sound and shipped it.

## Duplication, counted rather than subtracted

11 of the 320 member payloads are byte-identical to another, leaving **309
distinct**. Every duplicate is **inside a single archive**; **no payload is
shared between two archives on this disc**.

| payload | copies |
|---|---|
| `SOUNDW.LZ` `BCKSND4` = `BCKSNDA` = `BCKSNDD` | 3 |
| `SOUNDW.LZ` `BCKSND5` = `BCKSNDB` = `BCKSNDE` | 3 |
| `SOUNDW.LZ` `BCKSND6` = `BCKSNDC` = `BCKSNDF` | 3 |
| `TORREY_P.CRS` `PAN6` = `PAN7` | 2 |
| `TORREY_P.CRS` `OBJ29` = `OBJ30` = `OBJ31` = `OBJ32` = `OBJ33` | 5 |

So the sixteen background sounds are **ten**, and five of the thirty-four
course objects are the same sixteen-byte placeholder. Both figures were counted
by hashing 320 payloads, not obtained by subtracting one total from another.

## `GOLFER_F.LZ` against `GOLFER_M.LZ`

The pre-briefing picked these two out as the likeliest pair to share members:
37,240 and 40,400 bytes, built fifty seconds apart, identical count bytes.

| member | F raw | M raw | identical |
|---|---:|---:|---|
| `PUTTER.BLK` | 13,936 | 13,120 | no |
| `GOLFER.BLK` | 37,360 | 41,472 | no |
| `CHIPPER.BLK` | 13,920 | 14,592 | no |

**3 of 3 names shared, 0 of 3 payloads shared.** The same structure holding
different art, which is what a male and a female golfer should be.
