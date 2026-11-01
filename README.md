# Dictionary mark — design examples

A visual playground for choosing how a saved "dictionary word" should be marked inside an
e-reader page, without being confused with the sentence currently being read aloud.

**This repo holds design examples only. No application code lives here.**

---

## The problem

Two things get marked inside body text:

1. **Read aloud** — while a sentence is being read aloud, it gets a **2px solid underline** running
   under the whole sentence. This is fixed; it cannot move or change.
2. **Dictionary word** — a word the user saved to their personal dictionary gets its own mark.

Today the dictionary mark is a 2px dashed underline drawn at the bottom of the text's content box,
which lands about **5 CSS px below** the read-aloud line. The two merge into one thick double
underline, so the saved word no longer stands out.

## Hard constraints (measured on the real device)

- Body text: **16px Literata, line-height 1.4** (22.4px).
- Content box of a text run: **24px tall** — font ascent 19px, descent 5px.
- **Baseline is 19px from the top** of that box; the box bottom is the descender line.
- The read-aloud solid line sits **at the box bottom**.
- A dictionary mark may sit anywhere **from the baseline upward**. Above the baseline it starts
  cutting through descenders (g, y, p); below the box bottom it collides with the next line.
  In practice there is only **~5px of room** between the baseline and the read-aloud line.
- The mark is drawn on an **absolutely positioned div that wraps exactly one word's fragment on one
  line** — known width, 24px height, no text inside it. So the mark must be pure CSS on an empty
  box: `border`, `background-image` (gradients), `box-shadow`, `outline`, or `::before`/`::after`.
- **No JavaScript** in the mark, no images, no external resources.
- **Pastel background highlights are already used for quoted text**, so a background wash is not
  available for the dictionary mark.
- The mark must also work in **vertical writing modes** (the rule runs down the side of the word
  instead of under it).
- Reader font size and line spacing are user-configurable, so marks built from `em` units are
  preferred over fixed pixels.

## What has already been tried

- 2px dashed underline at −3px, −5px, −6px above the read-aloud line: always reads as one thick
  double line, because the whole available band is only 5px.
- 1.5px dotted rule at −5px: better, but still reads as "a second line" at normal reading size.

## Suggestions / directions worth exploring

- Dots of varying weight and spacing (fine vs coarse), square vs round.
- A short, heavy "highlighter tick" covering only part of the word.
- Double rules, or a rule with a gap at each end.
- A mark in an accent colour instead of the text colour (biggest visual separation — treat as a
  design decision, not a default).
- Something that is **not a line at all**: a small dot after the word like a footnote marker, a
  single dot centred underneath, dots above the word, a tiny superscript badge.
- Anything that makes the mark readable at 100% zoom, not just when magnified.

## What this repo should contain

A single self-contained `index.html` gallery that renders every candidate **in real context**:

- one realistic English paragraph (novel prose, no lorem ipsum);
- one sentence inside it carrying the **2px solid read-aloud underline**;
- several saved words inside that sentence, each with a different candidate mark;
- each candidate shown at **100% size** (how a reader actually sees it) and again at **2x and 4x**
  zoom, so both the real-world impression and the pixel detail are visible;
- a numbered legend naming each candidate in a few words;
- a short recommendation section at the bottom.

Everything inline in the one file: inline `<style>`, no external fonts, no network requests, no
images. It must render identically in Chrome and Android WebView.
