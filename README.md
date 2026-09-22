**English** · [Español](README.es.md)

# RSVP — Rapid Serial Visual Presentation

**A repository about RSVP (Rapid Serial Visual Presentation): what it is, why it works, and how it can be applied to on-screen reading.**

This repo will gather material and tools around this reading technique. For now it includes a complete, working **web reader**; the rest is documentation about the concept itself.

---

## What is RSVP?

**RSVP** stands for *Rapid Serial Visual Presentation*, a technique for presenting information in which stimuli are shown **one at a time, at the same point on the screen**, at a controlled pace.

Applied to reading, it means showing one word at a time, always in the same position, instead of presenting a block of text that the eye has to scan. The reader doesn't move their eyes across the page: the words appear where they're already looking.

It's the basis of many "speed reading" apps and has also been used in research on attention, working memory, and visual perception.

---

## Why does it work?

### 1. It eliminates saccadic movements

When we read normal text, our eyes don't glide continuously: they advance in jumps (**saccades**) and stop briefly at each point (**fixations**). Every jump costs time and attention, and sometimes forces a step back (regressions).

RSVP removes that cost: there are no jumps or regressions because there's nowhere to move. The word arrives at the fixation point.

### 2. It anchors each word to its ORP

The **ORP** (*Optimal Recognition Point*) is the position within a word where the eye identifies it fastest. It sits near the beginning of the word (usually the 2nd–3rd letter, depending on length).

If a word is simply centered, the eye has to correct its position to land on that point. If the **ORP** is aligned with the center of the screen, recognition is immediate. That's why good RSVP readers highlight that letter and shift the word so it sits on the axis.

### 3. It makes pace explicit

In normal reading, pace depends on text difficulty and attention. In RSVP it's set with a parameter (**WPM**, words per minute). That allows you to:

- Read **faster** than saccades would allow.
- Read **slower and more focused** when the text is dense.
- Keep a **steady rhythm**, with no acceleration or braking.

Good RSVP readers also **modulate** that pace: they give more time to long words and to punctuation, so the reading doesn't feel mechanical.

### 4. It reduces visual load

With no surrounding text, there's less information competing for attention. The eye has a single target and the brain doesn't spend resources filtering neighboring lines.

---

## What is it for?

- **Speed reading** of articles, emails, reports, or notes.
- **Reviewing** material you already know.
- **Focused study**: forcing a pace prevents drifting.
- **Accessibility**: useful for some people with visual tracking difficulties.
- **Research**: it's a classic paradigm in experimental psychology.

### When it's not ideal

- Text where **visual structure** matters (tables, code, diagrams, formulas).
- Material that requires **re-reading** or comparing passages.
- Texts where **formatting** (bold, lists, hierarchy) is part of the meaning.
- When the goal is **deep comprehension**, not speed.

RSVP speeds up access to information; it doesn't replace attentive reading when that's what's needed.

---

## The ORP, in detail

The ORP is usually calculated from the length of the word, counting only letters and numbers (punctuation doesn't count):

| Length | ORP position |
| --- | --- |
| 1 | 0 |
| 2–5 | 1 |
| 6–9 | 2 |
| 10–13 | 3 |
| 14–17 | 4 |
| 18–21 | 5 |
| 22–25 | 6 |
| 26+ | 6 + ⌊(n − 25) / 4⌋ |

The idea is that the pivot never lands too far to the left on very long words, but doesn't drift so far right that the tail grows without bound either.

In serious implementations, the pivot **skips punctuation**: if a word starts with `"`, `(`, `¿`, or contains internal symbols, the ORP must land on a real letter or number, never on a symbol. This matters especially with URLs and tokens that contain punctuation.

---

## Pacing: why a constant speed isn't enough

An RSVP reader that always advances at the same interval feels artificial. The brain doesn't process a three-letter word and a fifteen-letter one the same way, nor a comma and a period.

A common scheme (and the one used by the reader in this repo) is:
```
interval = (60000 / WPM) × multiplier
```

Where `multiplier` starts at `1` and adds:

- `+0.15` if the word has more than 8 letters/numbers.
- `+0.15` more if it has more than 12.
- `+0.80` if it ends in `. ! ? …`.
- `+0.40` if it ends in `, ; :`.

This makes the text "breathe" where it should and keeps sentences from tripping over each other.

---

## Repository contents

For now, this repo contains:

- **`index.html`** — a complete RSVP web reader, in a single file, with no dependencies and no build step.

### The web reader

A single-page RSVP reader that can be opened directly in the browser or served from GitHub Pages.

**Reading**
- ORP highlighted in red and anchored to the center of the stage.
- Automatic font size adjustment by measuring the real text width (`canvas.measureText`).
- Adjustable WPM from 60 to 1200.
- Natural pauses based on length and punctuation.
- *Chunking* of very long tokens (URLs, compound words) so they never run off screen.

**Loading text**
- Paste directly into the textarea.
- Drag and drop files.
- Upload multiple files at once.
- Support for many extensions: `.txt`, `.md`, `.rst`, `.log`, `.csv`, `.json`, `.html`, `.xml`, `.css`, `.js`/`.ts`/`.tsx`, `.py`, `.rb`, `.go`, `.rs`, `.java`, `.c`/`.cpp`, `.sh`, `.yml`, `.toml`, `.ini`, `.sql`, `.tex`, and more.
- Automatic cleanup of HTML/XML/SVG (tags, scripts, and styles are stripped).

**Playback**
- Play/pause, ±1 and ±10 words.
- Clickable progress bar.
- Estimated time remaining.
- Current / total word counter.
- WPM is persisted in `localStorage`.

**Export and sharing**
- Video export with `MediaRecorder` + `canvas` (tries MP4; falls back to WebM if the browser doesn't support it).
- Shareable link generation with the text and WPM compressed into the URL hash (`#v1=…`).

---

## Contributing

Issues and PRs are welcome. Any improvement to the reader, correction to the docs, or additional reference on RSVP is appreciated.

---

