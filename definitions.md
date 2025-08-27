Got it. Below is a structured, plain-English definition for every function in your file, grouped by section. I describe purpose, inputs, outputs, side effects, and any important notes.

# Types, Advantage Matrix, and Art

### `artFaction(theme)`

* **Purpose:** Produce SVG markup for Faction cards, with subtle variation by theme.
* **Inputs:** `theme` (`"realistic"` or `"fantasy"`).
* **Returns:** A string of SVG markup.
* **Side effects:** None.
* **Notes:** Adds an extra decorative circle in fantasy mode.

### `artBadge(theme)`

* **Purpose:** Produce SVG markup for Badge cards.
* **Inputs:** `theme`.
* **Returns:** SVG string.
* **Side effects:** None.

### `artTicket(theme)`

* **Purpose:** Produce SVG markup for Ticket cards.
* **Inputs:** `theme`.
* **Returns:** SVG string.
* **Side effects:** None.

### `artLife(theme)`

* **Purpose:** Produce SVG markup for Life Event cards.
* **Inputs:** `theme`.
* **Returns:** SVG string.
* **Side effects:** None.

### `artEconomy(theme)`

* **Purpose:** Produce SVG markup for Economy cards.
* **Inputs:** `theme`.
* **Returns:** SVG string.
* **Side effects:** None.

### `svgFor(type, theme)`

* **Purpose:** Dispatch to the correct art generator based on a card type.
* **Inputs:** `type` (from `TYPES`), `theme`.
* **Returns:** SVG string for that type.
* **Side effects:** None.
* **Notes:** Centralizes art selection so card builders can remain simple.

# RNG (Seedable Randomness)

### `mulberry32(a)`

* **Purpose:** Create a small deterministic PRNG function from a 32-bit seed.
* **Inputs:** `a` (integer seed).
* **Returns:** A function that yields a float in `[0,1)`.
* **Side effects:** None.
* **Notes:** Used to make shuffling and randomizers reproducible.

### `applySeed(seedStr)`

* **Purpose:** Switch randomness to a deterministic PRNG based on a text seed, or revert to default randomness.
* **Inputs:** `seedStr` (string; empty or falsy clears the seed).
* **Returns:** `undefined`.
* **Side effects:** Sets the module-level `rand` function to either `Math.random` or a seeded generator; logs the change.

### `die()`

* **Purpose:** Roll a six-sided die using the current RNG.
* **Inputs:** None.
* **Returns:** Integer from 1 to 6.
* **Side effects:** None.

### `rpsThrow()`

* **Purpose:** Produce a Rock/Paper/Scissors throw using the current RNG.
* **Inputs:** None.
* **Returns:** One of `"rock"`, `"paper"`, or `"scissors"`.
* **Side effects:** None.

# Bank Actions and Populators

### `populateBankActions()`

* **Purpose:** Fill the BANK action `<select>` with the available spend options.
* **Inputs:** None.
* **Returns:** `undefined`.
* **Side effects:** Mutates the DOM options of `#bankAction` based on `BANK_ACTIONS`.

# UI Helpers

### `setTurnText()`

* **Purpose:** Update the on-screen turn indicator based on `turn`.
* **Inputs:** None.
* **Returns:** `undefined`.
* **Side effects:** Writes to `#turnIndicator` text.

### `announce(msg)`

* **Purpose:** Push an assertive, screen-reader-friendly announcement of a game event.
* **Inputs:** `msg` (string).
* **Returns:** `undefined`.
* **Side effects:** Writes to the visually hidden `#liveAnnouncer` node for accessibility.

### `log(msg)`

* **Purpose:** Append a line to the visible log and announce it for accessibility.
* **Inputs:** `msg` (string).
* **Returns:** `undefined`.
* **Side effects:** Appends to `#log`, auto-scrolls it, and calls `announce`.

### `cardHTML(card)`

* **Purpose:** Render a card’s HTML (art, name, tags) for display in stacks, hands, and battlefield.
* **Inputs:** `card` (object from the deck).
* **Returns:** HTML string for a card tile.
* **Side effects:** None.
* **Notes:** Shows type, power, score or bank modifiers, and a target chip for forced targets.

### `renderStack(id, cards)`

* **Purpose:** Render an array of cards into a container (deck or discard).
* **Inputs:** `id` (container element id), `cards` (array).
* **Returns:** `undefined`.
* **Side effects:** Replaces the container’s inner HTML with card tiles.

### `renderHand(idx)`

* **Purpose:** Render one player’s hand and wire up playable cards with click and keyboard handlers.
* **Inputs:** `idx` (0 for Player A, 1 for Player B).
* **Returns:** `undefined`.
* **Side effects:** Replaces the hand container’s contents; attaches event listeners on playable cards; sets ARIA attributes for accessibility.

### `renderAll()`

* **Purpose:** Redraw deck, discard, both hands, and scores in one call.
* **Inputs:** None.
* **Returns:** `undefined`.
* **Side effects:** DOM updates via `renderStack`, `renderHand`, and `updateScoreboard`.

### `updateScoreboard()`

* **Purpose:** Synchronize the SCORE and BANK numbers on the UI with in-memory state.
* **Inputs:** None.
* **Returns:** `undefined`.
* **Side effects:** Updates `#scoreA`, `#bankA`, `#scoreB`, `#bankB`.

# Deck Construction

### `expandLife(theme)`

* **Purpose:** Expand Life Event specs into concrete card objects, including repeated copies.
* **Inputs:** `theme`.
* **Returns:** Array of Life Event card objects.
* **Side effects:** None.

### `expandEconomy(theme)`

* **Purpose:** Expand Economy specs into concrete card objects, including repeated copies and opponent-targeted entries.
* **Inputs:** `theme`.
* **Returns:** Array of Economy card objects.
* **Side effects:** None.

### `buildDeck(theme)`

* **Purpose:** Build the full deck by combining base cards, expanded Life Event cards, and expanded Economy cards, with per-type SVG art.
* **Inputs:** `theme`.
* **Returns:** Array of all card objects.
* **Side effects:** None.

# Shuffle and Deal

### `shuffleInPlace(arr)`

* **Purpose:** Shuffle an array uniformly using Fisher-Yates with the current RNG.
* **Inputs:** `arr` (array to shuffle).
* **Returns:** The same array, now shuffled.
* **Side effects:** Mutates the array in place.

### `dealEightEach()`

* **Purpose:** Deal 8 cards to each player from the top of `deck` and start the round.
* **Inputs:** None.
* **Returns:** `undefined`.
* **Side effects:** Empties 16 cards from `deck` into `players[0].hand` and `players[1].hand` alternately; sets `turn` to Player A; clears battlefields; enables round-related buttons; writes to the log and UI.

# Randomizers and Combat Resolution

### `rngMode()`

* **Purpose:** Read the selected randomizer mode from the UI.
* **Inputs:** None.
* **Returns:** A string value such as `"off"`, `"die-tie"`, `"die-always"`, `"rps-tie"`, `"rps-always"`.
* **Side effects:** None.

### `rpsCompare(a, b)`

* **Purpose:** Compare two R/P/S throws.
* **Inputs:** `a`, `b` (each `"rock"`, `"paper"`, or `"scissors"`).
* **Returns:** `1` if `a` beats `b`, `-1` if `b` beats `a`, `0` on tie.
* **Side effects:** None.

### `hasTypeAdvantage(a, b)`

* **Purpose:** Determine whether either card has a defined rock-paper-scissors style advantage.
* **Inputs:** `a`, `b` (card objects).
* **Returns:** `true` if either direction shows an advantage, else `false`.
* **Side effects:** None.

### `baseCompare(a, b)`

* **Purpose:** Resolve a clash without randomizers using matrix rules, badge trumping, and power tiebreakers.
* **Inputs:** `a`, `b` (card objects).
* **Returns:** `1` if `a` wins, `-1` if `b` wins, `0` if tie.
* **Side effects:** None.
* **Notes:** Applies Badge vs Faction and Badge vs named trumps first, then the cyclic advantage, then same-type power, then cross-type power.

### `compareWithRandomizers(a, b)`

* **Purpose:** Resolve a clash applying base rules, then optional randomizer logic depending on the current mode.
* **Inputs:** `a`, `b` (card objects).
* **Returns:** `1`, `-1`, or `0` for win, loss, or tie.
* **Side effects:** Writes randomizer details into the log.
* **Notes:** If any type advantage exists, it resolves without randomness. Otherwise, it uses die or R/P/S as a tiebreaker or as the primary method depending on mode.

# PvP, Draw, and Core Play

### `pvpToggle()`

* **Purpose:** Read whether generic Life/Economy effects invert toward the opponent.
* **Inputs:** None.
* **Returns:** `true` or `false`.
* **Side effects:** None.

### `drawN(p, n)`

* **Purpose:** Draw `n` cards for player `p` from `deck`.
* **Inputs:** `p` (0 or 1), `n` (positive integer).
* **Returns:** `undefined`.
* **Side effects:** Removes up to `n` cards from `deck` into `players[p].hand`; logs if the deck empties; re-renders that player’s hand.

### `playCard(playerIdx, handIndex)`

* **Purpose:** Execute the act of playing a card from a player’s hand and advance the turn, including:

  * applying Life and Economy effects (respecting targets and PvP inversion),
  * placing Faction/Badge/Ticket cards onto the battlefield,
  * resolving a clash when both battlefields are occupied,
  * moving resolved cards to discard,
  * checking end-of-round conditions.
* **Inputs:** `playerIdx` (0 or 1), `handIndex` (index into that player’s hand).
* **Returns:** `undefined`.
* **Side effects:** Mutates hands, scores, bank values, discard, `bf`, `turn`; updates multiple UI regions; appends to the log.
* **Notes:** Contains an inner helper `applyBank(targetIdx, delta)` that applies positive deltas as reductions to the target’s BANK and negative deltas as increases, with logging.

# Round and Score Management

### `resetScores()`

* **Purpose:** Reset both players’ round SCORE to zero.
* **Inputs:** None.
* **Returns:** `undefined`.
* **Side effects:** Updates in-memory scores and UI; logs the reset.

### `nextRound()`

* **Purpose:** Start a new round if enough cards remain, optionally auto-resetting scores based on the checkbox.
* **Inputs:** None.
* **Returns:** `undefined`.
* **Side effects:** May zero out scores, deals fresh hands, re-renders, and logs the new round. Leaves BANK values intact.

# Controls and Utilities

### `enable(el, on)`

* **Purpose:** Toggle a control’s enabled state and synchronize its `aria-disabled` attribute.
* **Inputs:** `el` (DOM element), `on` (boolean).
* **Returns:** `undefined`.
* **Side effects:** Mutates button disabled state and ARIA attribute.

# Event Wiring (for completeness)

These are not regular functions but are important to state flow:

* **Build Deck button handler:** Builds a themed deck, clears state, populates BANK actions, resets battlefields and turn, re-renders, enables Shuffle, disables actions that require a hand, and logs the deck size.
* **Shuffle button handler:** Shuffles the current deck using `shuffleInPlace`, re-renders, and enables Deal.
* **Deal button handler:** Deals 8 cards each (if no hand is active), re-renders, logs, and starts with Player A.
* **Next Round button handler:** Calls `nextRound`.
* **Reset Scores button handler:** Calls `resetScores`.
* **Spend BANK button handler:** Executes the selected BANK action for the current player if sufficient BANK exists, then logs and updates the scoreboard.
* **Apply Seed button handler:** Reads the seed text, calls `applySeed`, and logs seed status.
* **DOMContentLoaded boot:** Programmatically clicks “Build Deck” so the page is ready immediately.

---

If you want, I can transform this into inline JSDoc comments in the source, or export a separate Markdown developer guide that mirrors these definitions and includes a state diagram of the turn flow and clash resolution.
[https://chatgpt.com/share/68aeff40-0270-800c-ac85-44a8b5d3d190](https://chatgpt.com/share/68aeff40-0270-800c-ac85-44a8b5d3d190)

