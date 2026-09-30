# Mixermind

A browser-based code-breaking puzzle game — crack the hidden four-color
combination in ten guesses. Inspired by the classic board game *Mastermind*,
built from scratch as a single-file HTML/CSS/JavaScript app.

**▶ [Play it here](https://Zebaztian2525.github.io/mixermind/)**

---

## How to play

The computer has picked a secret sequence of four colors (duplicates allowed).
Your job is to figure it out before you run out of tries.

1. **Drag** a color from the palette onto any slot in the current row, or
   **click** a color and then click a slot.
2. Click a filled slot to clear it, or use **Undo** to remove the last color.
3. Press **Guess** when all four slots are filled.

After each guess you get feedback:

| Marker | Meaning |
|--------|---------|
| ⚫ Black | Correct color in the correct position |
| ⚪ White | Correct color, but in the wrong position |

Markers are not tied to specific slots — they only tell you *how many* of each.

You have **10 guesses**. Good luck.

---

## Features

- Six colors, four positions, ten attempts
- Drag-and-drop **or** click-to-place — works with mouse and touch
- Classic black/white feedback pegs
- Undo, new game, and a clean dark UI
- Single self-contained HTML file — no build step, no dependencies
- Responsive down to phone size

---

## Running locally

Just open `index.html` in any modern browser. That's it.

To host it yourself on GitHub Pages:

1. Push `index.html` (plus this README and a `LICENSE`) to a public repo.
2. Go to **Settings → Pages**.
3. Set *Source* to **Deploy from a branch**, branch `main`, folder `/ (root)`.
4. Save. Your game will be live at `https://DITTNAMN.github.io/mixermind/` in
   about a minute.

---

## Tech

Vanilla JavaScript, no frameworks. Drag-and-drop is implemented with custom
pointer events (`mousedown`/`touchstart` + move tracking) rather than the
HTML5 Drag API, so the same code handles desktop and mobile without the usual
quirks.

The feedback algorithm handles duplicate colors correctly:

```js
function feedback(guess, secret) {
  let black = 0;
  const secretLeft = {}, guessLeft = {};
  for (let i = 0; i < secret.length; i++) {
    if (guess[i] === secret[i]) {
      black++;
    } else {
      secretLeft[secret[i]] = (secretLeft[secret[i]] || 0) + 1;
      guessLeft[guess[i]]   = (guessLeft[guess[i]]   || 0) + 1;
    }
  }
  let white = 0;
  for (const c in guessLeft) {
    white += Math.min(guessLeft[c], secretLeft[c] || 0);
  }
  return { black, white };
}
