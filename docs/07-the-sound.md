# 07 — the sound: eighty-nine seconds of a commentator, and one chime from `C:\WIN31\`

*Measure: `SOUNDW.LZ` holds **97 RIFF WAVE files, 97 of 97 parsing**, 96 at
11,025 Hz mono 8-bit and one at 22,050 Hz. 988,479 bytes of PCM, **89.134
seconds**, two independent duration routes agreeing to 0.000000000 s. Nineteen
members are stored uncompressed and play as they sit on the disc. 91 of the 97
payloads are distinct.*

---

## What came out

    python tools/mdmd.py _work/iso/SOUNDW.LZ --extract _work/members/SOUNDW.LZ
    -> extracted 97 members, 992747 bytes
    python tools/wavcheck.py _work/members/SOUNDW.LZ

    files matching .wav            : 97
    parsed as RIFF/WAVE           : 97
    not RIFF/WAVE                 : 0
    format tags                   : 1 x97
    A  data / nAvgBytesPerSec     : 89.133968254 s
    B  data / frame / rate        : 89.133968254 s
    |A - B| over the population   : 0.000000000 s
    nAvgBytesPerSec != rate*frame : 0 files
       tag    ch       Hz  bits    files
       1       1    11025     8       96
       1       1    22050     8        1

The decoder was not trusted on its own account: 97 files decoded by the LZW
implementation in [05-the-codec.md](05-the-codec.md) produce 97 well-formed
RIFF containers whose chunk walks account for every byte on 96 of 97, and whose
two duration formulas agree exactly. **A wrong decompressor does not produce
that.**

The one exception is `BRIAN.WAV`, whose chunk walk reaches 2,082 bytes in a
2,081-byte file: an odd-length chunk needing a pad byte that was never written.
It is a defect in the 1990 file, faithfully preserved, and it is filed in
[13-leftovers.md](13-leftovers.md).

## The naming rule, closed twice over

The 97 names split into three groups with different shapes.

**Twelve female/male pairs.** `BELIEVE`, `DIDTREE`, `GIMMIE`, `GOHOLE`,
`HATEIT`, `HAVEONE`, `HURRY`, `NICETRY`, `OHOH`, `SLOW`, `WASTREE`, `YES` —
each present as `...F.WAV` and `...M.WAV`.

The owner of this machine listened and reported that `BELIEVEF` and `BELIEVEM`
are the same line — *"I can't believe I did that!"* — in a female and a male
voice. The measurement agrees without anyone listening again: **mean duration
1.009 s for the twelve female clips against 0.954 s for the twelve male ones**,
and eleven of the twelve pairs within a factor of 1.42 of each other.

The suffix is the **gender of the voice**, and it runs in parallel with the two
sprite archives, `GOLFER_F.LZ` and `GOLFER_M.LZ`, which share all three of
their member names and none of their bytes. The game has a male and a female
golfer and it has a male and a female line for each thing a golfer says.

**Sixteen background sounds**, `BCKSND0` through `BCKSNDF`. They are 0.323 to
1.268 s with a coefficient of variation of **0.2752**, against **0.7214** for
the other eighty-one — a factor of 2.62. A tight duration band is what a set of
interchangeable ambient beds looks like, and the naming does not need to be
believed to see it.

They are also **not sixteen**. `BCKSND4` = `BCKSNDA` = `BCKSNDD`, `BCKSND5` =
`BCKSNDB` = `BCKSNDE`, `BCKSND6` = `BCKSNDC` = `BCKSNDF`, byte for byte. **Ten
distinct beds, shipped as sixteen entries.**

**Sixty-nine event clips**, named for what happens: `ACE`, `GRETBIRD`,
`GRETEAGL`, `GRETPAR`, `GRETPUTT`, `GRETDBLE`, `NICEBIRD`, `NICEPAR`,
`NICEPUTT`, `NICESHOT`, `INROUGH`, `INSAND`, `INTREES`, `ONBEACH`, `SPLASH`,
`OUTAHERE`, `CRUSHIT`, `RATTLE`, `PINHIT`, `SITDOWN`, `GETUP`, `GETLEGS`,
`BITE`, `MOREBALL`, `STILLOUT`, `TOOMUCH`, `WHATPUTT`, `GOODSAVE`, `THATSWET`,
`THATPLAY`, `SHTSTUFF`, `DPSTUFF`, `DANCIN`, `GIFT`, `QUACK1`, `TREE1`,
`TREE2`, `SWING`, `PUTT`, `PUTT1`, `CHIP`, `SAND`, `CLAPLOUD`, `CLAPMED`,
`CLAPPOLI`, `GOODBYE`, `PRACTICE`, `ADLIB`, `BECLUB`, `GETTHERE`, `GOTAHOLD`,
`GOTTAHIT`, `STLTURN`, `BRIAN`, `DING` and the pairs above.

This is a **golf commentator**, and the vocabulary is the vocabulary of one:
the outcomes, the lies, the applause at three volumes, and a goodbye.

## The chime from `C:\WIN31\`, on three independent witnesses

`DING.WAV` is unlike every other member of this archive in three ways that were
measured separately and only afterwards lined up:

1. it is the **only member at 22,050 Hz**; the other 96 are 11,025;
2. its `MDmd` node header carries the build path **`C:\WIN31\`**; the other 96
   carry `WAV\` or nothing, and no other node on the whole disc names a Windows
   directory;
3. the owner of this machine played it and identified it, unprompted, as the
   **standard Microsoft Windows 3.1 system chime**.

Three witnesses, three methods, one conclusion: somebody building a DOS golf
game for a Modular Windows console reached into their Windows directory for a
notification sound and shipped it inside the game's own archive, at its native
sample rate, without resampling it to match the other ninety-six.

It is a very small fact and it is the most specific thing this disc says about
the machine it was made on.

## `BRIAN`, and what it is not

`BRIAN.WAV` is a personal name and it appears on a disc that Access Software
made and Tandy sold, so it is P.1 published material and is treated as such in
[12-privacy.md](12-privacy.md).

**It is a name in a filename and nowhere else.** The owner of this machine
played it: it is the sound of a club striking a ball, 2,081 bytes, 1.24
seconds, and there is no voice in it and no name spoken. Whoever `BRIAN` was —
a developer, a colleague who swung a club into a microphone, an in-joke — the
disc does not say, and this repository does not guess. It is recorded because
it is there, at the granularity at which it is there: **a member name, not a
credit.**

## How much of the disc this is

`SOUNDW.LZ` is 891,212 bytes as stored, **39.0140 %** of the disc's file bytes,
and it is the largest single file on it. Decoded, the 97 members are 992,747
bytes of which 988,479 are PCM sample data.

That figure is the disc's thesis number and it is worked in
[11-the-thesis.md](11-the-thesis.md) over four denominators, because 39 % and
43 % and 25 % are all true of this disc and mean different things.

## What the MS-DOS release had instead

The MS-DOS release of the same game ships `sounds.lz`, 92,406 bytes, 36
members. **Zero of its member names appear in `SOUNDW.LZ` and zero of its
payloads do.** All 97 sounds on this disc are new for the VIS.

That is the single largest change in the port. The course is the same bytes,
the lie tables are the same bytes, the top view is the same bytes — and the
sound was rebuilt from nothing at roughly ten times the size, because the
target had a CD-ROM and 8-bit PCM speech is exactly the thing you spend a
CD-ROM on. See [10-against-the-collection.md](10-against-the-collection.md).
