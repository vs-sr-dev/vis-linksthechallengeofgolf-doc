# 08 — `GOLF.EXE`: a real-mode DOS program that names 82.7 % of the disc's members

*Measure: 157,780 bytes, MZ, `(309 − 1) × 512 + 84 = 157,780` with residue 0.
268 relocations across **3 distinct segments**, all 268 landing inside the load
image. No second header of any kind; `ne.py` and `pe.py` both refuse it. **270
printable runs of six characters or more, all 270 read.** The string table names
**263 of the 318 distinct member names in the nine archives** — verbatim or
through three literal templates the program also carries.*

---

## It is a DOS program and there is no arguing with it

    python tools/mz.py _work/iso/GOLF.EXE

    e_cp / e_cblp              309 / 84
    (309-1) * 512 + 84         157,780  ==  the file length, residue +0
    e_cparhdr                  96 paragraphs = 1,536 bytes
    e_crlc                     268 relocations at 0x1C, 1,072 bytes
    relocations inside image   268 / 268
    distinct reloc segments    3
    header slack after table   436 bytes, all zero
    e_minalloc / e_maxalloc    0 / 1
    cs:ip / ss:sp              16AE:553E / 165A:0540
    e_lfanew at 0x3C           380,509,147

The field at 0x3C is not a pointer to anything. The relocation table begins at
0x1C and runs straight through 0x3C, so a Windows loader reading `e_lfanew`
reads two relocation entries. Both structural readers say so and both are
right:

    python tools/ne.py _work/iso/GOLF.EXE
      no NE signature at e_lfanew=380509147 (found b'')
    python tools/pe.py _work/iso/GOLF.EXE
      ValueError: no PE signature at 0x16AE1BDB

`NE` appears five times in the file and `LE` eight times, none of them a
header; `PE\0\0` appears zero times. **There is no second executable format on
this disc.**

And the strings settle it independently of every header field. These end in
`$`, which is how `INT 21h` function `09h` terminates a string and is not how
anything else in computing does:

    Golfer patchX=$      Golfer patchZ=$      holeNumber   =$
    Golfer Xcoor =$      Golfer Zcoor =$      CTRL/ALT/ESC pressed...$

## What the 261 unread strings said

The pre-briefing had nine of the 270 runs transcribed. All 270 have now been
read. The interesting ones fall into six groups.

**A build stamp, four minutes before the file's own timestamp.**

    ' LINKS VIS version 1.00  '
    '  10-22-92 at 16:46      '

`GOLF.EXE`'s ISO 9660 recorded time is 1992-10-22 16:50:58. The program was
linked at 16:46 and written to the master image at 16:50:58 on the same day.
**Two clocks four minutes apart**, which is what a build script looks like.

**A memory requirement, which is a platform fact.**

    'Initial memory allocation error!'
    'Sorry... Not able to continue.'
    '    LINKS requires at least 530K bytes of free memory to'
    '    operate properly, and your machine has only xxxK bytes free.'
    'Memory deallocation error'
    'EMMXXXX0'

**530 K of free conventional memory**, and `EMMXXXX0` is the device name a DOS
program opens to ask whether an expanded-memory manager is present. Both belong
to a real-mode DOS world. What that implies about the console is *not* settled
here — see [09-the-game-test.md](09-the-game-test.md) and
[10-against-the-collection.md](10-against-the-collection.md).

**A keyboard.**

    '1234567890-='   'QWERTYUIOP[]'   "ASDFGHJKL;'~"   '/ZXCVBNM,./'

Four rows of a US keyboard, laid out as a scan-code map. A VIS shipped with a
remote control.

**Its own error messages about the containers.**

    'Course INDEX missing'    'Sound Index missing'    'LINKS.CFG missing'
    'Exit from LINKS...'      'Error A'                'Error F'

`~INDEX~` is the name of the directory node at the top of seven of the nine
archives. The program calls it an index and fails by name when it is absent.

**Two settings, in words.**

    'UPSCALING ENABLED'      'UPSCALING DISABLED'
    'BALL TRACER ENABLED'    'BALL TRACER DISABLED'

**A debug artefact in a retail program.**

    'UUdebug.dmp'    'Thinking...'    'Please stand by...'

## The bridge between the two halves of the disc

The pre-briefing set up one cheap cross-check — *`GOLF.EXE` contains the string
`DROP2.BLK`; is it a member of an archive?* — and did not run it. It is, of
`GOLF1.LZ`. Run properly, the check is much bigger than one string.

    distinct member names across the nine archives : 318
    named verbatim inside GOLF.EXE                 :  89  (28.0 %)
      plus covered by the template 'PATCHxx.PAT'   : 135
      plus covered by the template 'OBJxx.BLK'     :  34
      plus covered by the template 'BCKSND0.WAV'   :  16
    verbatim or templated                          : 263  (82.7 %)
    neither                                        :  55

**The program carries the templates as literal strings**, next to the names
they generate:

    'PATCHxx.PAT'      135 members named PATCHnn.PAT
    'OBJxx.BLK'         34 members named OBJnn.BLK

So the disc explains its own structure, and it does it in two registers: fixed
assets by name, numbered series by template. The 55 names in neither group are
**all commentary WAVs** — `ACE`, `BELIEVEF`, `GRETEAGL`, `HURRYM` and so on —
which are selected at run time by index out of the sound index rather than
opened by name. That is not a gap in the accounting; it is the accounting
telling you how the sound system works.

Names the program carries that are **not** on this disc:

    'OBJECTS.LZ'    'MAINPNL0.CEL'    '$TEES.BLK'

`OBJECTS.LZ` does not exist here; the objects live inside `TORREY_P.CRS` as
`OBJ00.BLK`..`OBJ33.BLK`. `.CEL` appears nowhere on this disc — but
`PLOTBALL.CEL` and `SCORCARD.CEL` are members of the **MS-DOS** release's
archives. The VIS build carries string-table residue naming a file layout it no
longer uses.

## The compiler, which is not identified

*Sherlock Holmes*' `SHI.EXE` was identified as Borland C++ 1991 from strings.
The same pass on `GOLF.EXE` finds **no compiler or runtime signature at all**.
Nineteen needles, counted rather than eyeballed, every one of them zero:

    Borland  BORLAND  Turbo  TURBO  Microsoft  MICROSOFT  Watcom  WATCOM
    Zortech  Lattice  runtime  Runtime  RUNTIME  'abnormal program'
    'stack overflow'  Divide  Access  ACCESS  Copyright  COPYRIGHT

and the regular expression `19[0-9][0-9]` matches **zero times in 157,780
bytes**. There is no year anywhere in this program.

This is a stripped, self-contained real-mode binary with three relocation
segments and a hand-rolled `INT 21h` string interface. **The toolchain is not
identified and is not guessed.** Clause C32 predicted an identification was
reachable and that it would not be Borland; the second half is unfalsified and
the first half is wrong.

## The overlay question, answered in the negative

`e_minalloc = 0` and `e_maxalloc = 1` are the shape of a program that asks DOS
for no extra memory beyond its image, which usually means either a packer or an
overlay scheme. The pre-briefing correctly called that **a reading, not a
measurement**, and asked for evidence beyond the two header fields.

The evidence is against it. There is no overlay manager string, no `.OVL`
reference, no second load module, and the 268 relocations resolve across only
**3 segments** — a small, flat program image. The file is not packed: its
entropy is 4.4799 bits/byte, which is ordinary 8086 code, and 261 of its 270
printable runs are in clear.

What actually explains `maxalloc = 1` is on the previous page: the program
tells the user it needs **530 K free** and then allocates what it needs itself.
It is not asking DOS for the largest free block; it is checking the number and
refusing.

## The comparison that is coming

The MS-DOS release's `golf.exe` is **60,950 bytes with 88 printable runs** —
roughly a third the size and a third of the strings.

**`[corrected]` — the launcher reading is withdrawn, and the string count never
supported it.** The next repository did open those files, and:

```
python ../pc-linksthechallengeofgolf-doc/tools/entropy.py        Links_The_Challenge_Of_Golf --tree
  golf.exe   60,950   H 7.9680        the highest of that object's 32 files
```

**`golf.exe` is packed.** Its 88 printable runs are noise from a compressed
image, not a small program's small string table, and **0 of that release's 285
archive member names occur in any of its six executables** where this build
names 263 of 318. Re-tested in this session on the four of the six that are
Microsoft EXEPACK images and therefore unpackable: **0 of 285 in the unpacked
load images either.** `golf.exe` is not among the four and is packed by
something still unidentified.

> **A count of printable runs in a compressed file measures the compressor.**
> The ratio 88 : 270 is not evidence about how the two releases divide their
> code, and no conclusion should be drawn from it in either direction.

**Where the rest of the MS-DOS program lives is therefore reopened**, not
answered. The `.lz` files beside it (`select.lz`, `playset.lz`, `ready.lz`,
`selctcrs.lz`, `selctplr.lz`, `selpract.lz`, `info.lz`, `golfer2.lz`) remain
the candidate and remain unopened from this side.

**The VIS build folded what the MS-DOS release splits across `golf.exe` and its
`.lz` modules into one 157,780-byte image** — restated, because the previous
wording said *the launcher and the modules* and there is no launcher reading
left to lean on. That is consistent with a console with no configuration step, no
`setblast.exe` and no `systype.exe` — see
[10-against-the-collection.md](10-against-the-collection.md) — and it is why
searching the VIS binary for feature words is not a fair test of the two
releases against each other. `MULLIGAN` and `REPLAY` appear in neither
executable, and on a disc whose interface is drawn bitmaps that is not evidence
of absence.
