# 09 — the G test: a criterion that only works on one binary format is a criterion about binary formats

*Measure: `vis-gamelist-doc`'s four-part test asks four good questions and
names, in all four of its "where the answer lives" columns, a structure that
only Win16 NE binaries have. `GOLF.EXE` is a real-mode DOS MZ and has none of
them. This chapter proposes an amendment that relocates the evidence and leaves
the questions alone, then answers all four — **4 of 4** — on the amended
evidence.*

**This chapter proposes. It does not edit `vis-gamelist-doc`.** The text to be
copied there is in [10-against-the-collection.md](10-against-the-collection.md).

---

## The test as written, and why it cannot be run

    G1  the program presents a bounded set of options and branches on which
        one is taken
        -- where the answer lives: menu, dialog and string RESOURCES; one
           WINDOW CLASS per screen in the EXPORT TABLE
    G2  an attempt is scored right or wrong, or won or lost
        -- strings or RESOURCES naming outcomes, and more than one terminal
           state
    G3  something other than the player's own pace can end the attempt
        -- a timer, a clock, a score threshold or an antagonist in the
           program's own structures, not in an asset name
    G4  difficulty or content advances through named stages
        -- a level table, numbered stages, distinct content per stage

It was written against `SHI.EXE` and `RTC.EXE`, the two binaries this
collection had. **Every one of the four evidence columns names an NE
structure**: resources, export tables, window classes.

`GOLF.EXE` is a 157,780-byte real-mode MZ. It has no resource table, no export
table, no window classes, no segment table and no entry table. Queried as
written, the test returns nothing on the first disc in this library that is
unambiguously a game — which is a fact about the test.

## The amendment

The questions are good. **Only the evidence column needs rewriting**, and it
needs rewriting so that it names *what kind of evidence* rather than *which
container holds it*. Proposed replacement:

| | question (unchanged) | where the answer lives — **amended** |
|---|---|---|
| **G1** | the program presents a bounded set of options and branches on which one is taken | any enumeration the program can only have written to be chosen from: a resource menu, **a string table listing mutually exclusive labels**, **a drawn interface screen whose options are legible**, a dispatch table |
| **G2** | an attempt is scored right or wrong, or won or lost | any vocabulary of outcomes the program distinguishes: resource strings, **a string table naming terminal states**, **a scoring artefact the program fills in**, **outcome-indexed assets** |
| **G3** | something other than the player's own pace can end or cost the attempt | any rule the program enforces against the player independent of when they act: a timer, an opponent, **a penalty the program applies**, **an out-of-bounds or failure condition with its own text** |
| **G4** | difficulty or content advances through named stages | any enumeration of distinct content units: a level table, **numbered asset series with per-stage data**, **a stage index the program addresses by number** |

**Three rules go with it**, and they are the point:

1. **Name the evidence class, not the container.** "Resources" is where Win16
   keeps enumerations; it is not what an enumeration is.
2. **A drawn interface counts, and must be read rather than assumed.** On a
   1992 console the menu is frequently a bitmap. Rendering it is a measurement;
   inferring it from a filename is not.
3. **An asset name alone still does not count**, which is the original test's
   sharpest clause and survives unchanged. `INSAND.WAV` is not evidence of a
   sand penalty. The program's own text saying `IN THE SAND` is.

## The test, run on the amendment

### G1 — bounded options, branched on — **yes**

From `GOLF.EXE`'s string table, mutually exclusive labels the program prints:

    'PLAYER 1' 'PLAYER 2' 'PLAYER 3' 'PLAYER 4'
    'PLAYER 5' 'PLAYER 6' 'PLAYER 7' 'PLAYER 8'
    'UPSCALING ENABLED'   / 'UPSCALING DISABLED'
    'BALL TRACER ENABLED' / 'BALL TRACER DISABLED'
    'Exit from LINKS...'

From the drawn interface, rendered and read
([06-the-pictures.md](06-the-pictures.md)):

* **`SELPLAY.MLD`** — `PLAY: [18 Holes] [Front 9] [Back 9]`, then
  `PRACTICE: [1]…[18]`, then `[Cancel]`. A bounded set with a cancel path.
* **`NUMPLRS.MLD`** — *"How Many Players?"*, buttons `1 Player` … `6 Players`,
  `Cancel`.
* **`CDDSP.MLD`** — *"Do you wish to use this course?"*, `Yes` / `No` /
  `Cancel`.
* **`QUITCONF.BLK`** — named in the string table: a quit confirmation.

Every one of those is a set of choices the program had to be able to branch on.

### G2 — scored, won or lost — **yes**

The program's own text names a complete vocabulary of golf outcomes:

    'IN THE HOLE!'   'ON THE GREEN'   'IN FAIRWAY'    'IN THE ROUGH'
    'IN THE SAND'    'IN HAZARD!'     'ON CART PATH'  'OUT OF BOUNDS'
    'TO THE PIN'     'TO MARKER'      'IN THE AIR'

and the disc carries a **scoring artefact the program fills in**: `SC.MLD`, a
320 × 200 scorecard with holes 1–9 and `Out`, holes 10–18 and `In`, a running
`Tot`, and a `Par` row against every hole. `TOPMAP.MLD` states `PAR 72` and
three course ratings.

More than one terminal state, scored against a published par: this is the
clearest G2 in the collection.

### G3 — something other than the player's pace — **yes**

This is the criterion the original test was rightest to insist on, and the one
a golf simulation looks least likely to satisfy. It does, twice.

**The program applies penalties, in its own words:**

    'ONE PENALTY STROKE-YOU MAY REHIT'
    'NOW OR DROP ON NEXT TURN.'
    'ONE PENALTY STROKE-YOU MUST REHIT'
    'FROM POINT OF LAST SHOT.'
    'BALL IN HAZARD'  'BALL OUT OF BOUNDS'
    'YOU MUST REHIT FROM' 'POINT OF LAST SHOT'

A stroke added by the program against the player's will, and a forced replay
from a location the player did not choose, are costs the player's own pace
cannot avoid. That is the substance G3 asks for.

**And the disc enforces pace of play as content.** `SOUNDW.LZ` ships
`HURRYF.WAV`, `HURRYM.WAV`, `SLOWF.WAV`, `SLOWM.WAV` — a commentator with a
male and a female voice telling the player they are too slow. Under rule 3
above, asset names alone are *not* evidence; they are listed here as
corroboration of the penalty strings, not as the finding.

The honest edge of this answer: **nothing found forces the attempt to *end*
against the player's will.** Penalties cost strokes; the round is still
completed at the player's pace. G3 as amended asks for "end **or cost**", and
that widening is a change to the criterion which is being proposed openly
rather than smuggled. On the original wording — *"can end the attempt"* — the
honest answer for this disc is **no**, and that is stated so `vis-gamelist-doc`
can decide which wording it wants.

### G4 — named stages — **yes**

    'holeNumber   =$'      a variable the program prints by name
    'PATCHxx.PAT'          a template, and 135 members named PATCH00..PATCHG7
    'OBJxx.BLK'            a template, and 34 members named OBJ00..OBJ33
    'PAN0.BLK' .. 'PAN8.BLK'   nine panorama strips, named individually
    'LIE00.BLK' .. 'LIE05.BLK' six lie tables, named individually

and, drawn: `TOPMAP.MLD` shows **eighteen numbered holes** on a plan of the
property, and `SELPLAY.MLD` offers practice on each of the eighteen by number.
`SC.MLD` has a `Par` cell for each. Distinct content per stage, addressed by
number, in three independent places.

## The verdict, and the caveat that goes with it

**Amended G1–G4: 4 of 4.** *Links: The Challenge of Golf* is a game by
`vis-gamelist-doc`'s own criteria, and it is the first VIS disc in this
collection to satisfy all four.

**On the original wording of G3 it is 3 of 4**, because the disc's penalties
cost strokes rather than terminating the round.

Two things this chapter deliberately does not do.

It does not claim the amendment is *neutral*. Widening G3 from "end" to "end or
cost" makes the test easier to pass, and a test that gets easier when it meets
a disc it wants to admit is a test worth suspecting. **Both scores are
published so the maintainer of that repository can choose.**

And it does not use the amendment to relitigate the two discs already scored.
*Sherlock Holmes* and *Race the Clock* were assessed on NE evidence that
genuinely exists in their binaries; the amendment adds evidence classes rather
than removing any, so their answers cannot get worse. Whether they get better
is for that repository to run, not this one.
