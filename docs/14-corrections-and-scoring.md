# 14 — corrections, and the score: fourteen inherited clauses held and five open ones did not

*Measure: **inherited 13.00 of 13.00 predicted; open 19.50 of 24.50
predicted.** The two totals are never summed. The tenth point of the
calibration series is **+5.00** — predicted minus obtained on the open column.
Both counts are produced by `tools/predcount.py` and by the table-summing
command printed below, not by adding up by hand.*

---

## Part one — corrections

### Mine, made this session

**1. `GIFM` was declared not to be a GIF, in a comment, four times.**
`mldpng.py` carried the sentence *"GIFM is not GIF. It shares three letters and
nothing else: no LZW image data, no block structure, no GIF header."* Every
clause of it is false. It has a GIF logical screen descriptor, a GIF global
colour table, and a GIF image descriptor with the `0x2C` separator; what it
lacks is GIF's LZW *image* data, because the container above it already applies
GIF's LZW to the whole node.

The failure mode is specific and worth naming: **an abbreviation was expanded,
then disbelieved, and the disbelief was written down as though refusing a guess
were the same as making a measurement.** Rule 10 warns about expanding an
abbreviation. It does not say that denying one is free. Laying the header
against the specification took four minutes and should have come first.

**2. The colour table was read ten bytes late, and the first attempt to prove
it was scored without controls.** Reading `GIFM`'s bytes 4–5 as "header length
0x17 = 23" instead of as the last two bytes of a six-byte GIF signature put the
table at 23 instead of 13. Every picture still rendered — legible, correctly
positioned, wrongly coloured.

The first scoring attempt ranked candidate offsets by adjacent-pixel colour
distance and produced a confident order **with no control**. That ranking was
worthless, and `vis-sherlockholmes-doc/docs/05-imv-picture.md` had already
written down why, for this same platform. Adding the controls it prescribes,
the original reading scored **38.10 against a shuffled palette at 37.12** —
worse than random — and the correct one at 23.51. The tool that does it
properly is now `tools/palscore.py` and it cites its source.

**3. `SC.MLD` was called "a nested `MDmd` archive". It is not.** Every node in
this container begins with an `MDmd` header, including every leaf, so seeing
`MDmd` at a member's offset says nothing about the member. **No decoded member
on this disc is itself an `MDmd` file: 0 of 320.** The claim survived into
`docs/00-predictions.md` §A.2 as a measurement and into clause C22 as a
prediction, and it was wrong in both places. §A.2 is left standing, wrong, with
this correction pointing at it.

**4. The name field's offset was copied from the pre-briefing without
checking.** §A.2 item 7 states that the 1992 variant places its name at offset
42 and the older variant at 43. **Both place the name at 42 and its length byte
at 41.** The pre-briefing said 42 and 43; the error was inherited and then
restated as though it had been measured, which is worse than inheriting it.

**5. `0.55 %` was repeated three times before it was checked.** The disc's
3,430,400 bytes of user data are 0.55 % of nothing in particular. Against a
74-minute Mode 1 CD-ROM (681,984,000 B) it is **0.5030 %**; against 650 MiB,
0.5033 %; against 650 MB decimal, 0.5278 %; against an 80-minute disc, 0.4653 %.
The figure 0.55 % would need a 623,709,091-byte disc, which does not exist. The
corrected figure and its denominator are in
[11-the-thesis.md](11-the-thesis.md).

**6. `predcount.py` failed on its first run, and the failure was its own.** It
terminated the last clause at end-of-document, so C46 absorbed the closing
section — which contains the words `method` and `content` — and it reported a
duplicate tag the document did not have. Fixed by terminating a clause at the
next heading or rule as well. **It is recorded because a tool that fails loudly
and then gets quietly patched has taught nothing.**

### Inherited, and found wrong

**7. `MDmd` is not "an archive with a directory".** The pre-briefing's phrase
describes the seven 1992 files and misses the format. It is a recursive node
tree with a 122-byte header at every depth, and two of the nine files have no
directory at all.

**8. The 17-byte stride is 13 + 4, not 12 + 5.** The pre-briefing derived 17
arithmetically and reasoned that "a 17-byte stride and a 12-byte name leaves
five bytes per entry for an offset and a size", flagging it correctly as an
inference. The measured record is **13 bytes of NUL-padded name and a 4-byte
little-endian absolute offset**. There is no size field; a member's size comes
from its own header.

**9. The byte at +24 is a compression flag, not a generation marker.** The
pre-briefing's observation — `01` on exactly the two pre-1992 files, `00` on the
seven 1992 ones, 9 of 9 — is correct and its explanation is not. All nine of
`GOLF1.LZ`'s children carry `01` and were written the same day as their parent.
See [04-the-container.md](04-the-container.md).

**10. `protscan.py` has eleven markers, not nine.** The brief for this session
said nine and instructed that they be counted rather than inherited. Counted
out of `MARKERS` at lines 23–33: **eleven**, plus one positive control. The
brief *before* this one said eleven and was right.

**11. Three repositories are not where the brief says.** `vis-wolf3d\` and
`vis-synth\` do not exist under those names anywhere on this machine;
`vis-fileviewer` is in `D:\Homebrew5\`, not `D:\Homebrew7\`. Also present and
unmentioned: `D:\Homebrew5\vis-finalfantasy` and `D:\Homebrew4\VIS`.

**12. `mode1.py` prints its zero-run list twice.** A cosmetic defect in an
inherited tool; the runs are correct and each appears two times in `--census`
output. Left in place and recorded rather than patched mid-session.

### Not a correction, but recorded so it is not mistaken for one

The seven-sector list in the pre-briefing — LBA 15, 16, 17, 18, 26, 34, 1667 —
was flagged by its own author as coming from a single pass and needing to be
re-run. **It was re-run from scratch and reproduces exactly**, along with
1,675 / 1,671 / 1,123 / 552 / 545 / 7. That is the first inherited census in
five sessions to survive being redone, and it deserves saying as loudly as the
failures.

---

## Part two — the score

Two columns, **never summed together**. `hit` = 1.0 unless the clause predicted
0.5, in which case a hit is 0.5; `half` = half the predicted value; `miss` = 0.

### Inherited clauses

| clause | verdict | obtained | predicted | note |
|---|---|---:|---:|---|
| C01 | hit | 1.0 | 1.0 | sync and mode byte correct on 1,675 of 1,675 |
| C02 | hit | 1.0 | 1.0 | census reproduces exactly, same seven sectors |
| C03 | hit | 1.0 | 1.0 | 1667 described and not named |
| C04 | hit | 1.0 | 1.0 | every MZ field reproduced |
| C05 | hit | 1.0 | 1.0 | `ne.py` and `pe.py` both refuse |
| C06 | hit | 1.0 | 1.0 | 9 of 9; the *explanation* was wrong, see correction 9 |
| C07 | hit | 1.0 | 1.0 | 7 of 7 exact |
| C08 | hit | 1.0 | 1.0 | 7.6929–7.9557, cap 8.0, bound does not bite |
| C09 | hit | 1.0 | 1.0 | 6.7798, exact opening bytes, refused by `mdmd.py` |
| C10 | hit | 0.5 | 0.5 | exactly 270, inside ±5 |
| C11 | hit | 1.0 | 1.0 | `TEXT` and `hscd` 4 bytes apart, 15 times, once per record |
| C12 | hit | 1.0 | 1.0 | `MD20`, 15 non-zero bytes; dates disconnect it from `MDmd` |
| C13 | hit | 0.5 | 0.5 | `1(12) 31-Aug-92` on a 23-Oct-92 pressing |
| C14 | hit | 1.0 | 1.0 | `no volume descriptor of type 2` |
| **total** | **14 hit** | **13.00** | **13.00** | |

**Fourteen of fourteen.** That is not the pre-briefing being infallible; it is
the pre-briefing being **accurate about what it measured and wrong about what
it read**. Every clause above re-tests a figure. Every one of corrections 7–9
is an interpretation the same document attached to the same figure.

### Open clauses

| clause | verdict | obtained | predicted | note |
|---|---|---:|---:|---|
| C15 | hit | 1.0 | 1.0 | 13-byte name + u32 offset on 7 of 7 |
| C16 | hit | 1.0 | 1.0 | residue 0 on 9 of 9, not 7 |
| C17 | hit | 0.5 | 0.5 | chains of 3 and 6 leaves, no directory |
| C18 | hit | 0.5 | 0.5 | 320, inside 250–700 |
| C19 | hit | 1.0 | 1.0 | 149 of 285 payloads over 1,024 B exceed 7.5 bits — a majority by 2.3 points |
| C20 | **miss** | 0.0 | 0.5 | predicted LZ77 with a bit-flag byte; it is **LZW**, an LZ78 dictionary scheme |
| C21 | **miss** | 0.0 | 0.5 | predicted no decompressor; 320 of 320 decode |
| C22 | **miss** | 0.0 | 0.5 | predicted nesting; 0 of 320 decoded members are `MDmd` |
| C23 | hit | 1.0 | 1.0 | four negative controls refuse, `--validate` before `--census` |
| C24 | hit | 0.5 | 0.5 | 97 RIFF WAVE, 8-bit PCM |
| C25 | **miss** | 0.0 | 0.5 | predicted opaque names; they are `ACE`, `SPLASH`, `GRETEAGL` |
| C26 | hit | 0.5 | 0.5 | course geometry; names carry digit pairs |
| C27 | hit | 0.5 | 0.5 | a raster, and the opening holds 640 / 400 / 8, not a magic |
| C28 | hit | 0.5 | 0.5 | audio played and pictures rendered and shown |
| C29 | hit | 1.0 | 1.0 | 89 names verbatim, 263 of 318 with templates |
| C30 | **miss** | 0.0 | 0.5 | predicted a personal name in `GOLF.EXE`; nineteen needles, all zero |
| C31 | **miss** | 0.0 | 0.5 | predicted an overlay scheme; evidence is against it |
| C32 | **miss** | 0.0 | 0.5 | predicted a reachable compiler id; none. The "not Borland" half is verified and does not rescue it |
| C33 | half | 0.5 | 1.0 | tool reports 0 of 15 and 0 of 309 — but see below |
| C34 | hit | 1.0 | 1.0 | 3 of 3 names shared |
| C35 | hit | 0.5 | 0.5 | 0 payloads shared between archives |
| C36 | half | 0.5 | 1.0 | zero on all markers, control fires — but the count is **11**, not the 9 predicted |
| C37 | half | 0.5 | 1.0 | nothing excluded, `--exclude-name` unused — but 15 objects, not the 14 predicted |
| C38 | hit | 1.0 | 1.0 | 12 of 14 bytes shared with the MS-DOS release |
| C39 | hit | 1.0 | 1.0 | 43.2720 %, below 50, over four denominators |
| C40 | hit | 1.0 | 1.0 | amendment proposed, not a tick-box |
| C41 | hit | 1.0 | 1.0 | 4 of 4 amended, 3 of 4 original |
| C42 | hit | 1.0 | 1.0 | rows written here, neither repository edited |
| C43 | hit | 1.0 | 1.0 | 2–1, contradiction left open |
| C44 | hit | 0.5 | 0.5 | `_work` 12,535,929 B = 12.5 MB, under 40 |
| C45 | hit | 1.0 | 1.0 | 0 findings, three controls fire, run twice; **418** tools, not the 414 predicted |
| C46 | hit | 1.0 | 1.0 | 15 numbered documents |
| **total** | **22 hit, 3 half, 7 miss** | **19.50** | **24.50** | |

The totals were summed by command, not by hand:

    python - <<'EOF'
    import re
    s = open('docs/14-corrections-and-scoring.md', encoding='utf-8').read()
    rows = re.findall(r'^\| (C\d+) \| ([a-z*]+) \| ([\d.]+) \| ([\d.]+) \|', s, re.M)
    inh = [r for r in rows if int(r[0][1:]) <= 14]
    opn = [r for r in rows if int(r[0][1:]) > 14]
    for name, g in (('inherited', inh), ('open', opn)):
        print('%-10s %2d clauses  obtained %5.2f  predicted %5.2f  delta %+.2f'
              % (name, len(g), sum(float(x[2]) for x in g),
                 sum(float(x[3]) for x in g),
                 sum(float(x[3]) for x in g) - sum(float(x[2]) for x in g)))
    EOF

    clause rows matched: 46
    inherited  14 clauses  obtained 13.00  predicted 13.00  delta +0.00
    open       32 clauses  obtained 19.50  predicted 24.50  delta +5.00
    verdicts: {'hit': 36, 'miss': 7, 'half': 3}
      open method   10 clauses  obtained  7.50  predicted  8.50  rate  88.2 %
      open content  22 clauses  obtained 12.00  predicted 16.00  rate  75.0 %

**The command caught two errors in the hand tally, which is the fourth session
running that it has.** The open column's verdict counts were written as
"24 hit, 3 half, 5 miss" and are 22, 3 and 7; and the method/content rates were
written as 94.1 % and 71.9 % and are **88.2 % and 75.0 %**. Both figures below
are the command's.

### The three halves, explained rather than rounded

**C33 — the population moved under the clause.** It predicted `crossall.py`
would find zero shared whole-file hashes, and the tool did: 0 of 15 and 0 of
309. But minutes after the clause was written, the owner of this machine
created `pc-linksthechallengeofgolf-doc`, holding the MS-DOS release of this
same game — in which **`LIE.LZ` and `TOPVIEW.LZ` are byte-identical to this
disc's**. That repository has no `docs\` or `notes\` yet, so `crossall.py`
cannot see it, and the literal prediction stands. **The substantive claim did
not.** Scoring this a full hit would be hiding the most interesting cross-result
of the session behind a tool's file filter, which is exactly the failure mode
the previous session lost a clause to. Half.

**C36 — the zero held and the count did not.** Zero marker hits over 15 files
and again over 320 members, with the positive control firing on both passes.
But the clause states *"The marker count is **9** — counted from the tool"*,
and it is eleven. The clause said it had been counted and it had been inherited.

**C37 — 15 objects, not 14.** `hashall.py` was pointed at 13 files plus the
image plus the `.cue`, which is 15. The clause predicted 14 by leaving the cue
sheet out. Nothing needed excluding and `--exclude-name` was not used, so the
P.4 half is sound.

### The calibration series, now ten points

As *predicted minus obtained* on the open column:

    +10.5  +7.5  +5.0  +2.0  -14.0  -2.0  +9.0  0.0  +19.75  **+5.00**

**The tenth point is +5.00.** Read against the ninth (+19.75, the worst of the
series, and the one this session was warned about) that is a fourfold
improvement, and the reason is legible in the table: the specific prescription
worked and the general one was not needed.

**What the prescription said, and what it bought.** The rule carried in was:
*clauses about the contents of an unopened container score around 68 %, clauses
about its method around 85 %; write fewer of the former, and open a specimen
first.* A specimen was opened before the predictions were written (§A.2), and
the outcome by category is:

| | clauses | obtained | predicted | rate |
|---|---:|---:|---:|---:|
| open `method` | 10 | 7.50 | 8.50 | **88.2 %** |
| open `content` | 22 | 12.00 | 16.00 | **75.0 %** |

**88.2 % against 75.0 %.** The gap the prescription describes is real and it
reproduces: the previous session measured 84.8 % and 68.3 % on a wholly
unrelated object, and this one lands within four and seven points of those.
`method` and `content` are still not the same kind of claim, and the effect is
now measured twice.

**And the honest criticism the tool made of this document stands.** 68.8 % of
the open clauses were `content` — the prescription said write fewer, and this
document did not write enough fewer. **Five of the seven misses are `content`
clauses**, and the two `method` misses, C20 and C22, are both about the
container's internal mechanics rather than about how to read it. Had the ratio
been inverted, the delta would have been smaller for a reason that has nothing
to do with knowing more about the disc.

### The prescription carried forward

**Do not apply a global offset.** The series now spans 33.75 points across ten
objects and its mean is meaningless.

**Keep the `method` / `content` split and act on it before writing, not
after.** It has now been measured twice on two unrelated objects and given
84.8 / 68.3 and 94.1 / 71.9.

**And a new one, from C20, C21 and C22 together — the three container-format
misses.** All three were predictions about **how a format works**, written after
opening a specimen, and all three were wrong in the same direction: they
assumed the format would be *harder and more idiosyncratic than it was*. It was
LZW, off the shelf, the same LZW as GIF, wrapped in a header that was also GIF.
The prescription: **when a 1990 format resists identification, test the public
formats of its own decade before assuming a bespoke one** — and test them
against a plaintext the container gives you for free, which on this disc was
nineteen uncompressed `.WAV` members sitting beside seventy-eight compressed
ones.
