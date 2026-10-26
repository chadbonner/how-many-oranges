# How Many Oranges?

A daily estimation puzzle. One question a day, the same one for every player:
*how many of one thing does it take to equal another?*

> How many macarons stacked on top of each other would be as tall as the Big Ben tower?

You are not meant to know the answer. You work it out from two things you can
picture — a macaron is about an inch, Big Ben is about 300 feet — and you are
scored on how close your **order of magnitude** lands, not on hitting the
number exactly. Land within 2.5× and it's a bullseye.

## Playing

Pick an age band on the way in. Everyone gets the **same question**, so scores
mean the same thing whoever sends them. The band changes only how much help
you get:

| Band | Hint | Input |
|---|---|---|
| 6–9 | Always shown | Slider |
| 10–12 | One tap away | Slider |
| 13+ | None | Type the number |

After answering you get a shareable score card, Wordle-style — the log number
line rendered as eleven squares, with 🟩 the answer and 🟥 where you landed.

## How it's built

The whole game is **one self-contained HTML file** — no build step, no
dependencies, no backend, no accounts. Open `index.html` locally and it works.
The only network request is to Google Fonts.

### The daily puzzle is a pure function of the date

There is no server handing out questions. Every valid pairing of objects is
enumerated and shuffled with a fixed seed, so day *N* always resolves to the
same question for everyone, anywhere. `EPOCH` in `index.html` sets which
calendar day is puzzle #1.

A seven-day unit rotation (`WEEK`) means you never get the same kind of
question two days running.

### The object database

About 186 objects near the top of `index.html`, each with real measurements
stored in SI (metres, kilograms, litres, seconds) and displayed in US
customary units.

```js
{n:"a sloth", p:"sloths", e:"🦥", f:1, h:0.6, w:5, fact:"A sloth can take a full month to digest one meal."}
```

- `n` — how it reads in a sentence · `p` — countable plural · `e` — glyph
- `f` — familiarity, 0 everyday → 2 famous · `h/w/v/t` — height m, mass kg, volume L, duration s

Two rules govern what goes in, and they are the reason the game can be trusted:

**Every value must have a definition behind it** — a rule of the game, a
published spec or runtime, a museum measurement, a recipe step, a physical
constant. Anything whose real-world range is wider than the bullseye tolerance
gets cut rather than averaged. A marathon can be two hours or six, so there is
no marathon.

**Height depends on orientation.** A paper clip is 33mm on its end and 1mm
lying flat — two different questions with two different answers. Objects carry
`ho` for their natural orientation and optionally `h2`/`h2o` for the other one.
The question screen discloses the orientation beneath the question whenever it
affects the answer.

To add an object, add a row. The generator picks it up automatically.

## Deploying

Any push to `main` publishes to GitHub Pages via
`.github/workflows/deploy.yml`. The site is the repository root.

The "can you beat it?" link in a shared score follows wherever the page is
hosted — it reads its own address when it is the top-level document.
