# Crowdmark Match Descrambler

Crowdmark's [pre-matched booklets](https://www.crowdmark.com/help/using-pre-matched-booklets)
come as a single PDF in no particular order. If students are assigned to rooms by
last name, that stack is awkward to print and hand out.

This script reorders the PDF so you can print it and split it by room:

1. Blank (extra) booklets come first.
2. Matched booklets follow, sorted by `LAST NAME, First Name`.

Each booklet's pages stay together and in order. Pages are only reordered, not
edited, so the QR codes are unaffected.

> **Vibecoded:** This whole project, including the script, this README and the
> example PDF, was vibecoded with assistance from [Claude](https://claude.com/claude-code).
> Check the output before you print a whole term test.

## Usage

```
pip install pypdf
python3 sort_booklets.py booklets.pdf                # writes booklets-sorted.pdf
python3 sort_booklets.py booklets.pdf -o sorted.pdf
```

The script prints where each booklet ended up, which helps when dividing the stack
by room. Try it on the included example, which has 10 fictional booklets for famous
mathematicians and 2 blanks:

```
$ python3 sort_booklets.py example-booklets.pdf
pp.     1-2      #11     (blank)
pp.     3-4      #12     (blank)
pp.     5-6      #1      BERNOULLI, Jacob
pp.     7-8      #10     BERNOULLI, Johann
pp.     9-10     #7      DU CHÂTELET, Émilie
pp.    11-12     #9      EULER, Leonhard
pp.    13-14     #6      GAUSS, Carl Friedrich
pp.    15-16     #2      GÖDEL, Kurt
pp.    17-18     #5      KOVALEVSKAYA, Sofia
pp.    19-20     #4      MIRZAKHANI, Maryam
pp.    21-22     #8      NOETHER, Emmy
pp.    23-24     #3      RAMANUJAN, Srinivasa

10 matched + 2 blank booklets -> example-booklets-sorted.pdf
```

Accents, capitalization and punctuation don't affect the order.

## What your PDF needs

The script reads the text of each page, so it expects:

- A `#N Page k of K` header on every page, as Crowdmark stamps it.
- The labels `First (Given) Name:` and `Last (Family) Name:` on each booklet's first
  page, with the names printed to their right. If your cover page words these
  differently, change `FIRST_NAME_LABEL` and `LAST_NAME_LABEL` at the top of the script.

If the header or labels are missing, or a booklet has missing pages, the script stops
with an error and writes nothing. That way a student's paper can't be mistaken for a
blank.

It has been run on one real export (736 booklets, 1,472 pages), so other Crowdmark
templates may need adjustments.

## Student privacy

Real Crowdmark exports contain student names, emails and student numbers, so don't
commit them. The included `.gitignore` excludes every PDF except `example-booklets.pdf`,
which is entirely synthetic and doesn't work with Crowdmark.

This is an independent project, not affiliated with or endorsed by Crowdmark.

## License

[MIT](LICENSE)
