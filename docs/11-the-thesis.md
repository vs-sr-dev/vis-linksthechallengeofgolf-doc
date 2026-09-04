# 11 — the thesis: the first VIS entry outside the nineties, and it is 43 % or 25 % depending on the question

*Measure: 97 RIFF WAVE members carrying **988,479 bytes of PCM sample data**,
89.134 seconds, of which **937,471 bytes and 84.507 seconds are distinct**.
Over the disc's four denominators that is between **23.7961 %** and
**43.2720 %**. There is no video on this disc at all.*

---

## The column, and why this disc breaks it

`vis-platformnotes-doc` and its satellites carry a running figure: how much of
a disc is recorded media rather than program. Two VIS discs were in it.

| year | figure | title | what the figure is |
|---|---:|---|---|
| 1992 | **96.4344 %** | *Sherlock Holmes: Consulting Detective* | 1 h 48 m of a bespoke format |
| 1992 | **95.8047 %** | *Race the Clock* | 1,208 AVI files |
| 2026 | 24.8524 % | *DISSIDIA FF / DISSIDIA DUELLUM* | an Android tree |
| 1992 | **see below** | ***Links: The Challenge of Golf*** | **89 seconds of speech** |

Both prior VIS discs are, structurally, **video discs with a program attached**.
This one is not. It has no moving picture of any kind: no AVI, no `.IMV`, no
FLI, no MPEG, not one frame. Its recorded media is 89 seconds of a golf
commentator.

## Four denominators, and they differ by a factor of 1.8

Rule 3 says name the denominator. *Race the Clock* published **three** figures
because 80.2861 % of that disc was copies of itself. This one publishes four,
because the container expands and the medium is nearly empty.

| denominator | bytes | PCM data | PCM as a share |
|---|---:|---:|---:|
| raw image (1,675 × 2,352) | 3,939,600 | 988,479 | **25.0908 %** |
| user data (1,675 × 2,048) | 3,430,400 | 988,479 | **28.8153 %** |
| file bytes (13 files as declared) | 2,284,337 | 988,479 | **43.2720 %** |
| expanded content (members decompressed + the four non-archive files) | 3,497,799 | 988,479 | **28.2600 %** |

And the same four against **distinct** content, since 6 of the 97 WAV payloads
are duplicates of another (the sixteen `BCKSND` beds are ten):

| denominator | distinct PCM | share |
|---|---:|---:|
| raw image | 937,471 | **23.7961 %** |
| user data | 937,471 | **27.3283 %** |
| file bytes | 937,471 | **41.0391 %** |
| expanded content | 937,471 | **26.8017 %** |

A fifth figure exists and is not the thesis figure but is worth having beside
it: **`SOUNDW.LZ` as it sits on the disc is 891,212 bytes, 39.0140 % of the
file bytes**, and is the largest single file on the disc. That number is what a
`ls -l` would tell you, and it is smaller than 43.2720 % because the archive is
compressed and larger than 28.8153 % because the disc is a third empty.

## Which number goes in the column

**43.2720 %**, on the file-bytes denominator, with **41.0391 %** as the
distinct-content figure beside it.

The reasoning, stated so it can be disagreed with: the other two VIS rows are
computed over what their discs' files declare, not over their raw images, and a
column is only a column if its rows share a denominator. On this disc that
choice is unusually consequential — the raw-image figure is 25.0908 %, nearly
half the file-bytes figure, because **a third of this disc is zeroes** and no
other disc in the table has that property.

So the column entry carries its denominator explicitly and the medium figure is
published next to it rather than buried:

    1992   43.2720 %   Links: The Challenge of Golf   (VIS)
                       of 2,284,337 declared file bytes;
                       41.0391 % distinct; 25.0908 % of the 3,939,600-byte
                       raw image, on a disc that is 0.55 % full.

## What the number means, which is not what the other two mean

96.4344 % and 95.8047 % say *these discs are recordings with an index*. The
program on them is a viewer.

43.2720 % says something else. This disc is a **simulation** — a terrain model
in 135 patches, a physics table in six lies, a 157,780-byte program that
computes shots — with a commentator bolted on. The 89 seconds of speech are the
part a CD-ROM made possible and a floppy did not: the MS-DOS release of the
same game shipped 92,406 bytes of sound in 36 members, and **not one of them
survives here**. All 97 clips were made for this pressing.

So the honest sentence, with the number in it:

> The one disc in the Tandy VIS library that is unambiguously a game is also
> the only one that is not mostly a recording — and the recording it does carry,
> 43.2720 % of its declared bytes and eighty-nine seconds long, is the one
> thing the port added that the 1990 original could not afford.

## The other thing the medium figure says

The three VIS discs, by how much of their medium they used:

| disc | user bytes | share of a 74-minute CD-ROM |
|---|---:|---:|
| *Sherlock Holmes* | 445,760,838 | 65.36 % |
| *Race the Clock* | 374,094,820 | 54.85 % |
| ***Links*** | **3,430,400** | **0.5030 %** |

That last figure is the one that will be quoted, so it is worth stating which
capacity it is against, because the answer moves:

| capacity | bytes | this disc |
|---|---:|---:|
| 74 min, 333,000 sectors × 2,048 | 681,984,000 | **0.5030 %** |
| 80 min, 360,000 sectors × 2,048 | 737,280,000 | 0.4653 % |
| "650 MB" decimal | 650,000,000 | 0.5278 % |
| 650 MiB | 681,574,400 | 0.5033 % |

**The pre-briefing's inherited 0.55 % is on none of them.** It would require a
623,709,091-byte disc, which is not a thing. This repository publishes
**0.5030 %** and names its denominator; the correction is recorded in
[14-corrections-and-scoring.md](14-corrections-and-scoring.md).

Separately from the medium: on its own 1,675 declared sectors the disc fills
67.04 % with files and leaves the rest at zero. See
[03-the-medium.md](03-the-medium.md), which also works out that there was room
for **866 more golf courses** on the same platter.
