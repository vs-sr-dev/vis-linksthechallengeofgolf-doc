# 03 — the medium: a chapter about emptiness, and one sector nobody can explain

*Measure: 1,675 sectors of MODE1/2352, an exact divisor, sync and mode byte
correct on 1,675 of 1,675. 1,671 declared, residue 4 of run-out. 1,123 sectors
carry a file, 552 carry nothing, and 545 of those 552 are all zero. **The disc
is 0.5030 % full**, against the 681,984,000 bytes a 74-minute Mode 1 CD-ROM
holds. One sector — LBA 1667 — fails its own error correction and
is not explained.*

For three sessions this series had no medium to write about. This one has a
medium and the medium's most striking property is that almost none of it was
used.

---

## The geometry, and it closes

    3,939,600 / 2,352 = 1,675.0000     exact
    1,675 x 2,048     = 3,430,400      user bytes

    python tools/mode1.py "Links - The Challenge of Golf (USA).bin" --validate
      EDC   vectorised == scalar reference   32/32
      EDC   computed   == stored             32/32
      ECC P vectorised == scalar reference   32/32
      ECC P computed   == stored             32/32
      ECC Q computed   == stored             32/32
      -> byte range 0..2063 for EDC and 12..2075 for ECC confirmed on this image

The validation runs on 32 sectors and confirms the byte ranges; the census then
runs on all 1,675. **This is the first VIS disc in the collection whose every
sector has been checked** rather than only its first.

The primary volume descriptor declares 1,671 logical blocks against 1,675
present. Four sectors of run-out at the end of a track is ordinary and the
image is not truncated. **The disc really is this small.**

## A third of it is nothing

The accounting was re-derived from scratch rather than inherited, because the
pre-briefing said in its own words that its list came from one pass:

| | sectors | share of the image |
|---|---:|---:|
| in the image | 1,675 | 100 % |
| covered by one of the thirteen files | 1,123 | 67.04 % |
| covered by nothing | 552 | **32.96 %** |
| — of those, user area all zero | **545** | 32.54 % |
| — of those, not all zero | **7** | 0.42 % |

**The re-derivation reproduces the inherited figures exactly**, including the
identity of all seven exceptions: LBA **15, 16, 17, 18, 26, 34** and **1667**.
That is the first time in five sessions an inherited census has survived being
run again, and it is worth saying so plainly rather than only reporting the
failures.

`mode1.py --census` independently reports **571** zero-user sectors, which is a
different number for a good reason: it counts every sector in the image, not
only the uncovered ones. **26 zero-user sectors sit inside a file extent** —
the tail padding of files whose length is not a multiple of 2,048, plus zero
runs inside the files themselves. 545 + 26 = 571. Two tools, two definitions,
one agreement, and the agreement is the check.

Six of the seven exceptions are ordinary filesystem furniture:

| LBA | what it is | non-zero bytes |
|---:|---|---:|
| 15 | system area, opens `MD20` | 15 of the first 37 |
| 16 | primary volume descriptor, `CD001` | — |
| 17 | volume descriptor set terminator | — |
| 18 | (within the descriptor area) | — |
| 26 | path table | 3 of 2,048 |
| 34 | root directory | 694 of 2,048 |

## Sector 1667, which is not explained

It sits alone: 509 zero sectors before it, seven after, no file within 500
sectors of it. Its user area holds **550 non-zero bytes beginning at offset
1,496** — a tail occupying the last quarter of the sector with nothing above
it. That much was inherited. What is new is why it is anomalous:

    python tools/mode1.py IMG --census
      P parity mismatches                1
      Q parity mismatches                1
      reserved field 2068..2075 non-zero 1 / 1675

**All three of those are the same sector, and it is 1667.** Its reserved field,
which the Mode 1 specification requires to be zero, reads
`48 64 36 ab 56 ff 7e c0`, and neither its P nor its Q parity checks out.

That is a specific and useful shape. **This is not a Mode 1 sector with odd
contents; it is a sector whose error-correction region holds something that is
not error correction.** The 550 bytes in the user area and the eight bytes in
the reserved field are high-entropy, unstructured, and continue into the ECC
region without a boundary.

What it is **not**, tested: it is not the tail of any of the thirteen files
(the last file ends at LBA 1157); it does not decompress under the disc's own
LZW; it contains no printable run of six characters or more; it is not a
directory record.

The honest reading is that it is **residue** — whatever was in the mastering
system's buffer when it wrote a sector it did not otherwise need. That is a
reading and not a measurement, and it is filed in
[13-leftovers.md](13-leftovers.md) rather than asserted here.

## `MD20`, and a two-character coincidence that means nothing

Sector 15 opens `4d 44 32 30` = `MD20`, in the sixteen-sector system area where
a mastering tool puts its own structures. The preparer field says
`MERIDIAN_DATA_CD_PUBLISHER` and `MD` is the obvious expansion.

The nine archives on this disc open `MDmd`. **The two share two characters and
nothing else, and the dates prove it**: `TOPVIEW.LZ` and `LIE.LZ` were built in
1990 and 1991, one and two years before this disc was mastered, and they are
byte-identical to files in the MS-DOS release. Their container cannot have been
written by the CD publisher that stamped sector 15 in October 1992.

Expanding an abbreviation is not a measurement. Neither is refusing to. The
dates are the measurement.

## The emptiness, and what would have fitted in it

A 74-minute Mode 1 CD-ROM holds 333,000 sectors. This disc uses 1,675.

    (333,000 - 1,675) x 2,048  =  678,553,600 bytes unused
    TORREY_P.CRS                =       782,788 bytes, one complete golf course
    678,553,600 / 782,788      =  866

**There was room on this disc for eight hundred and sixty-six more golf
courses** — and it shipped with one, together with a screen reading *"Please
insert a new Course CD."*

The arithmetic is a measurement. **Why** a 1992 publisher would press one course
onto half a per cent of a CD-ROM and build a disc-swap prompt for the rest is
not, and this repository does not have the evidence to answer it. The owner of
this machine offers the obvious reading — that additional courses were product
— and it is recorded here as his inference and labelled as one. Access Software
did sell add-on course disks for the MS-DOS *Links*, which is context and not
proof about this pressing.

What the disc itself supports, without inference: **the multi-disc architecture
was built, shipped, and drawn.** `GOLF.EXE` names `CDCHANGE.MLD` and
`CDDSP.MLD`; both render; one asks whether to use the course found on the disc
in the drive, and the other asks for a new one. See
[06-the-pictures.md](06-the-pictures.md).

## The System Use area

The root directory record at LBA 34 carries the byte sequences `TEXT` and
`hscd` in its System Use area, which is where Apple's ISO 9660 extensions live.
They are present; nothing on this disc reads them; and a Tandy console has no
use for them. Filed as inherited furniture from the mastering software, in
[13-leftovers.md](13-leftovers.md).

## No second namespace

    python tools/iso9660.py IMG --compare
    -> one namespace

There is no Joliet supplementary volume descriptor. Thirteen files, zero
directories, one ten-byte path table. Nothing on this disc has a long name.
