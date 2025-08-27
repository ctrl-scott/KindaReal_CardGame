# Card Game Starter — Developer Guide

This guide documents all functions and their responsibilities in the Card Game Starter project.  
It is grouped by functional sections of the source file for clarity.

---

## Types, Advantage Matrix, and Art

### `artFaction(theme)`
Produce SVG markup for Faction cards, themed differently for fantasy or realistic.

### `artBadge(theme)`
Produce SVG markup for Badge cards.

### `artTicket(theme)`
Produce SVG markup for Ticket cards.

### `artLife(theme)`
Produce SVG markup for Life Event cards.

### `artEconomy(theme)`
Produce SVG markup for Economy cards.

### `svgFor(type, theme)`
Select the appropriate SVG generator based on a card's type.

---

## RNG (Seedable Randomness)

### `mulberry32(a)`
Create a deterministic pseudo-random generator function from a 32-bit seed.

### `applySeed(seedStr)`
Apply or clear a seed to make game randomness reproducible.

### `die()`
Roll a six-sided die using the active RNG.

### `rpsThrow()`
Produce a Rock/Paper/Scissors throw using the active RNG.

---

## Bank Actions and Populators

### `populateBankActions()`
Fill the BANK action selector with available spend options.

---

## UI Helpers

### `setTurnText()`
Update the displayed turn indicator.

### `announce(msg)`
Send a message to screen readers via a hidden live announcer.

### `log(msg)`
Append a message to the log panel and announce it.

### `cardHTML(card)`
Render a card into HTML markup for display.

### `renderStack(id, cards)`
Render an array of cards into the specified stack (deck/discard).

### `renderHand(idx)`
Render a player hand, wiring up playable cards with click/keyboard actions.

### `renderAll()`
Redraw deck, discard, both hands, and scores in one call.

### `updateScoreboard()`
Synchronize score and bank values in the UI with game state.

---

## Deck Construction

### `expandLife(theme)`
Expand Life Event specs into actual card objects (with multiple copies).

### `expandEconomy(theme)`
Expand Economy specs into actual card objects (with multiple copies).

### `buildDeck(theme)`
Build the full deck (base + life + economy cards).

---

## Shuffle and Deal

### `shuffleInPlace(arr)`
Shuffle an array in place with Fisher-Yates.

### `dealEightEach()`
Deal 8 cards to each player and start a round.

---

## Randomizers and Combat Resolution

### `rngMode()`
Return the current randomizer mode (die, RPS, etc.).

### `rpsCompare(a, b)`
Return win/loss/tie between two RPS throws.

### `hasTypeAdvantage(a, b)`
Check if either card has a type advantage over the other.

### `baseCompare(a, b)`
Compare two cards using type rules, badge trump, and power values.

### `compareWithRandomizers(a, b)`
Resolve a clash with base comparison and randomizer logic.

---

## PvP, Draw, and Core Play

### `pvpToggle()`
Check whether PvP inversion for generic Life/Economy is enabled.

### `drawN(p, n)`
Draw `n` cards for player `p` from the deck.

### `playCard(playerIdx, handIndex)`
Play a card from a player's hand, applying effects, resolving clashes, and updating state.

---

## Round and Score Management

### `resetScores()`
Reset both players' round scores.

### `nextRound()`
Start a new round if enough cards remain, optionally resetting scores.

---

## Controls and Utilities

### `enable(el, on)`
Enable or disable a DOM control, updating ARIA attributes.

---

## Event Wiring

- **Build Deck button**: Resets state, builds deck, enables shuffle.
- **Shuffle button**: Shuffles the deck and enables dealing.
- **Deal button**: Deals 8 cards each and starts the round.
- **Next Round button**: Starts the next round.
- **Reset Scores button**: Resets scores.
- **Spend BANK button**: Executes the chosen bank action if affordable.
- **Apply Seed button**: Applies a reproducible RNG seed.
- **DOMContentLoaded boot**: Auto-builds a deck for immediate play.

---

## Notes

- **Game state** is stored in global variables: `deck`, `discard`, `players`, `turn`, `bf`.
- **Logging** provides both visible output and accessibility announcements.
- **BANK actions** are modular and can be extended by adding new objects to `BANK_ACTIONS`.
- **Randomizers** only activate if no type advantage is present.

