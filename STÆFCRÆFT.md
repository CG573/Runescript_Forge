# Stæfcræft — The Artificer's Notation System

> *"The rune does not enchant. The rune instructs. There is a difference — and knowing it is the whole of the craft."*
> — attributed to the artificer Wulfhere of the Copper School

---

## Overview

**Stæfcræft** (Old English: *stæf* — letter/mark, *cræft* — craft/skill) is the formal discipline of encoding magical instructions directly into physical objects using carved runes. Practitioners — called *Stæfcræfters* — treat enchantment not as a mystical bestowal of power, but as the authorship of a program: a sequence of conditional logic, operations, and typed values that the object executes at runtime.

The notation system used to design and document Stæfcræft bindings is called **Runescript**. A Runescript binding is composed of one or more **clauses**, each describing a conditional trigger and an operation to perform. Clauses chain together in sequence, and state passes between them through the return-value register ᛟ (*Ōs*).

This document is the complete reference for Runescript syntax, the rune lexicon, and the rules governing valid bindings.

---

## The Runescript Forge

The **Runescript Forge** is a web-based composition tool for writing and validating Stæfcræft bindings. It provides:

- A live rune lexicon organised by class
- A clause-based script builder with drag-and-drop reordering
- Real-time syntax validation with error and warning annotations
- Plain-English translation of every clause
- One-click copy of the script string or its translation

Access it at: `[your GitHub Pages URL]`

---

## Syntax

### Clause Structure

Every clause follows this pattern:

```
[CONDITIONAL][¬?] → [NOUN] [VERB] [NOUN?] [MODIFIER?]
```

- The **conditional** is optional. A clause without one fires unconditionally.
- The **negation operator ᚾ** (Niht) may follow a conditional to invert it. This is the only position where Niht acts as a logical operator rather than a scalar modifier.
- The **arrow** (→) separates the conditional from the body. It is implied by the syntax and written explicitly only in notation.
- Clauses are separated by **‖** (double pipe). Read it as "then."

A complete multi-clause script reads left to right, top to bottom. Clauses are numbered §1, §2, §3... in documentation.

### A minimal valid clause

```
ᚩ → ᛞ ᛚ ᚷ
```
*On hit — draw the life force from the struck target.*

One conditional. One verb. One noun. That is sufficient.

### A clause with no conditional

```
ᚠ ᛟ ᚹ
```
*Transfer the prior result to the wielder.*

No gate — fires every time execution reaches it.

---

## The Rune Lexicon

Runes belong to one of four **classes**. A valid clause body must contain at least one verb and one noun. Conditionals and modifiers are optional.

### Nouns — the subjects and objects of operations

| Rune | Name | Meaning |
|------|------|---------|
| ᛚ | Lif | Life force / HP pool |
| ᛒ | Blōd | Blood / living tissue specifically |
| ᛊ | Sāl | The soul / animating essence |
| ᚹ | Wīl | The wielder — always refers back to the user |
| ᚦ | Þing | The projectile itself |
| ᚷ | Gāst | A target struck by the projectile (post-impact) |
| ᚫ | Āna | The aimed-at target / target in the reticle (pre-impact) |
| ᛟ | Ōs | The return value of the prior verb — the result register |
| ᛠ | Tācn | The marked entity / bearer of the active Bind mark |

> **Gāst vs Āna:** This distinction matters. Gāst is only valid after a hit has occurred — it refers to something already struck. Āna is the target as seen through a scope or detection effect, before any impact. Using Gāst in a pre-hit clause is a semantic error the linter will not catch; it is the cræfter's responsibility.

> **Ōs:** Ōs is a reference, not a stored value. It holds whatever the immediately preceding verb produced. If the preceding clause was skipped (its conditional failed), Ōs is empty, and any clause consuming it should be gated with ᛝ (Ing) to avoid operating on a null register.

> **Tācn:** Tācn resolves to whichever entity currently carries a Bind mark written during this activation cycle. It is an entity handle, not a value — Fær can deliver *to* Tācn, Drān can pull *from* Tācn. If no Bind mark is active, Tācn is undefined; always gate clauses using Tācn with ᚸ (Mǣrk). Tācn, Ōs, and Mǣrk form a natural trio: **ᛈ Bind writes a mark** onto a noun, **ᚸ Mǣrk checks** whether any active mark exists, and **ᛠ Tācn references** the entity carrying it.

---

### Verbs — the operations

| Rune | Name | Meaning |
|------|------|---------|
| ᛞ | Drān | Draw / pull toward — extract something from a source |
| ᚠ | Fær | Transfer / move from one entity to another |
| ᚱ | Rād | Read / sense / detect — produces a value into Ōs |
| ᛈ | Wrītan | Attach / mark / flag — inscribes a persistent mark onto the target noun |
| ᛖ | Ec | Amplify / multiply — scales a value upward |
| ᛏ | Tām | Suppress / dampen — scales a value downward, or silences an effect |

> **Tām as annotation:** Tām applied as a scoring (physical overlap) over another rune during carving suppresses that rune's output permanently. This is how nerfed enchantments are implemented in the metal itself rather than in the script logic.

---

### Conditionals — the logic gates

| Rune | Name | Fires when... |
|------|------|---------------|
| ᚩ | Hrīnan | The projectile contacts a target |
| ᚳ | Þyrel | The projectile strikes a vital zone |
| ᚪ | Sēon | The wielder is actively sighting |
| ᛝ | Ing | The immediately prior clause succeeded (chain gate) |
| ᚸ | Mǣrk | There is an active Wrītan mark in this activation cycle |

#### Negation — ᚾ Niht

Any conditional may be negated by placing ᚾ (Niht) immediately after it, before the arrow. The combined form reads as the logical inverse:

| Form | Meaning |
|------|---------|
| `ᛝ → …` | If the prior clause succeeded |
| `ᛝᚾ → …` | If the prior clause did NOT succeed (else) |
| `ᚸ → …` | If there is an active Wrītan mark |
| `ᚸᚾ → …` | If there is NO active Wrītan mark |

This enables **if / else-if / else** branching across consecutive clauses:

```
ᚸ  → …     if marked
ᚸᚾ → …     else if (not marked, but something else holds)
ᛝᚾ → …     else
```

> **Niht elsewhere:** Outside of the negation position (immediately after a conditional), ᚾ functions as a scalar modifier meaning "inverted direction." A Drān modified by Niht pushes rather than pulls. Context determines which role Niht is playing.

---

### Modifiers — scalars and annotations

| Rune | Name | Effect |
|------|------|--------|
| ᛁ | Lyt | Small / fractional — approximately 25% of base output |
| ᛗ | Micel | Large / amplified — approximately 2× base output |
| ᚻ | Hwīl | Duration — the effect lingers over time rather than firing once |
| ᚾ | Niht | Inverted — flips the direction of a verb (when not used as negation) |

Modifiers follow the verb and nouns in the clause body. More than one modifier may be applied; they stack multiplicatively unless the enchantment's medium specifies otherwise.

---

## Validation Rules

The Runescript Forge enforces these rules automatically. Cræfters working from notation should apply them manually.

### Errors — the script cannot execute correctly

| Condition | Rule |
|-----------|------|
| Conditional not at position 0 | A conditional must be the first rune in a clause |
| Multiple conditionals in one clause | Each clause may have at most one conditional |
| ᛝ or ᚸ on §1 | These conditionals require a prior clause; §1 has none |
| Conditional with no body | A gated clause with no verb is a dead branch |
| Verb with no nouns | A verb has nothing to act on — the operation is undefined |

### Warnings — the script may not behave as intended

| Condition | Rule |
|-----------|------|
| Multiple verbs in one clause | Only the first verb executes; the rest are ignored |
| Modifier with no verb | The scalar has nothing to scale |

---

## Examples

### 1 — A Simple Ward Glyph

**Intent:** When struck, suppress the incoming force.

```
ᚩ → ᛏ ᛒ
```

| | |
|--|--|
| §1 | *Hrīnan — suppress the blood / living tissue* |

**Notes:** A single clause. No state, no chaining. The Tām rune dampens whatever force reaches living tissue on impact. Suitable for a shield boss or armour plate. The simplest possible Stæfcræft binding — one gate, one operation, one noun.

---

### 2 — A Detection Scope

**Intent:** While sighting, read the life force of whatever is in the reticle and display it to the wielder.

```
ᚪ→ᚱᛚᚫ ‖ ᛝ→ᚠᛟᚹ
```

| | |
|--|--|
| §1 | *Sēon — read the life force of the aimed-at target* |
| §2 | *If §1 succeeded — transfer the result to the wielder* |

**Notes:** §2's Ing gate is essential. Without it, §2 fires unconditionally — and if §1 read nothing (the wielder is sighting a wall), Ōs is empty, and the Fær has no value to deliver. The Ing gate makes §2 a null-safe consumer of §1's output. The wielder perceives the reading as a health aura on the target.

---

### 3 — A Vampyric Bolt

**Intent:** A crossbow bolt that drains life from its target and returns a portion to the shooter.

```
ᚩ→ᛞᛚᚷ ‖ ᛝ→ᚠᛟᚹᛁ
```

| | |
|--|--|
| §1 | *Hrīnan — draw the life force from the struck target* |
| §2 | *If §1 succeeded — transfer the result to the wielder, at reduced potency* |

**Notes:** Lyt on §2 nerfs the return — the full drain is extracted from the target, but the wielder only receives a fraction. The remainder disperses. This is the simplest vampyric binding. Note that Tām scoring over the Drān rune in the metal can further suppress the drain at the source; the script and the carving work together.

---

### 4 — A Scoped Primer (Scope-to-Barrel Handshake)

**Intent:** While sighting, mark the chambered projectile so the barrel knows the shot was scoped.

```
ᚪ → ᛈ ᚦ
```

| | |
|--|--|
| §1 | *Sēon — mark the projectile* |

**Notes:** A single-clause cross-component signal. The scope runs this binding continuously while the eye is at the glass. The Wrītan mark is inscribed onto the projectile (Þing) and persists until the projectile fires. The barrel's script reads this mark via ᚸ (Mǣrk) — which checks only whether an active mark exists, not what carries it. Hip-fired shots bypass the scope entirely — Sēon never fires — so no mark is written and Mǣrk returns false.

---

### 5 — A Marking Blade (Wrītan / Mǣrk / Tācn in practice)

**Intent:** On a critical strike, mark the target. On the next hit against any marked target, drain their life force.

```
ᚳ→ᛈᚷ ‖ ᚸ→ᛞᛚᛠ ‖ ᛝ→ᚠᛟᚹ
```

| | |
|--|--|
| §1 | *Þyrel — mark the struck target* |
| §2 | *If an active mark exists — drain the life force from the marked entity* |
| §3 | *If §2 succeeded — transfer the result to the wielder* |

**Notes:** This illustrates the Wrītan / Mǣrk / Tācn trio working as a unit. §1 inscribes the mark onto Gāst (the struck target) using Wrītan. §2 uses Mǣrk to check whether that mark is active — not which entity carries it — and then uses ᛠ Tācn to resolve the marked entity as the subject of the drain. §3 chains the result to the wielder. The mark persists across separate hits, making this pattern useful for two-weapon setups, delayed effects, or multi-round debuffs.

---

### 6 — The Vampyric Rifle (Full Pipeline)

**Intent:** A rifle with a vampyric-enchanted barrel and a life-detecting scope. The scope reads the target's health as an aura. When fired scoped, the drain is amplified and returned to the wielder as healing. When fired from the hip, only the base drain is returned.

```
ᚪ→ᚱᛚᚫ ‖ ᛝ→ᚠᛟᚹ ‖ ᚪ→ᛈᚦ ‖ ᚩ→ᛞᛚᚷ ‖ ᚸ→ᛖᛟᛁ ‖ ᛝ→ᚠᛟᚹ ‖ ᚸᚾ→ᚠᛟᚹ
```

| | |
|--|--|
| §1 | *Sēon — read the life force of the aimed-at target* |
| §2 | *If §1 succeeded — transfer the result to the wielder* |
| §3 | *Sēon — mark the projectile* |
| §4 | *Hrīnan — draw the life force from the struck target* |
| §5 | *If an active mark exists — amplify the prior result, at reduced potency* |
| §6 | *If §5 succeeded — transfer the result to the wielder* |
| §7 | *If no active mark — transfer the result to the wielder* |

**Execution paths:**

```
Hip-fire, miss:         §1–3 skip  §4 skip   §5–7 skip                   → nothing
Hip-fire, hits living:  §1–3 skip  §4 fires  §5 skip  §6 skip  §7 fires  → base heal
Scoped, aimed at wall:  §1 fires   §2 skip   §3 fires  §4 skip  §5–7 skip → aura only
Scoped, hits living:    §1 fires   §2 fires  §3 fires  §4 fires §5 fires  §6 fires  §7 skip  → aura + boosted heal
```

**Design notes:**

- §1–2 are the **scope module**: continuous life-detection feeding perception. Logically independent of the barrel.
- §3 is the **handshake**: Sēon writes a Wrītan mark onto the projectile (Þing). This is the only cross-component communication in the script.
- §4 is the **core barrel enchantment**: vampyric drain on impact. The Tām scoring on the Drān rune in the barrel metal suppresses the raw extraction to a non-lethal level — a physical annotation, not a script clause.
- §5 checks Mǣrk — *is there an active mark?* Since the only Wrītan in this script fires in §3, and §3 only fires while Sēon is active, the mark is present only on scoped shots. ᛠ Tācn is not needed here because §5 amplifies Ōs (a value), not the marked entity itself.
- §5–7 form the **routing fork**: ᚸ and ᚸᚾ are mutually exclusive by definition. Exactly one fires per hit that reaches §4. §6 depends on §5 via Ing, properly chaining the boosted path. §7 catches all unmarked hits and delivers the base drain directly.

This binding is physically split across two objects — the scope and the barrel — but written as a single unified script. The cræfter must ensure both components share the same attunement signature, or the Mǣrk check in §5 may respond to marks written by other active bindings in proximity.

---

## Carving Notes for Practitioners

**Barrel helices:** Interior rune-rifling runs breach-to-muzzle. The bullet reads the script as it travels the barrel length; mechanical spin activates each glyph in sequence. A rune carved muzzle-to-breach inverts its own meaning — this is a common source of catastrophic misfires in apprentice work.

**Scoring:** Overlapping one rune partially over another during carving suppresses the underlying rune's output without removing it from the script. This is how Tām is most commonly applied — as a physical annotation rather than a logical clause. A scored rune is still visible and still present in the notation; it is written with a strikethrough in manuscript tradition.

**Buffer registers:** Crystalline materials (particularly translucent minerals with internal fault-line channels) function as persistent Ōs buffers. A scope stone does not just transmit the Rād result — it holds it briefly, long enough for the paired Fær to consume it on the next instruction cycle. Polished crystal loses this property; the natural fault lines are the conductive medium.

**Cross-component bindings:** When a single script spans multiple physical objects, the objects must be attuned to the same activation signature during the carving ritual. Unattached components read Wrītan marks from any source — a second marked weapon in proximity will contaminate the Mǣrk check. Professional artificers use a unique attunement mark (an additional Wrītan sub-glyph specific to that weapon pairing) to scope the check.

---

## Glossary

| Term | Definition |
|------|-----------|
| **Stæfcræft** | The discipline of encoding magical instructions into physical objects via carved runes |
| **Runescript** | The notation system used to design and document Stæfcræft bindings |
| **Binding** | A complete Runescript program carved into an object |
| **Clause** | A single conditional-and-operation unit within a binding |
| **Ōs register** | The implicit return-value slot that passes the output of one clause to the next |
| **Tācn reference** | The implicit entity handle that resolves to whatever currently carries an active Wrītan mark |
| **Scoring** | The practice of carving one rune over another to suppress its output |
| **Handshake** | A Wrītan-and-Mǣrk pair used to pass state between two physically separate components |
| **Stæfcræfter** | A practitioner of Stæfcræft |
| **Dead branch** | A clause whose conditional can never fire — usually an Ing gating a clause whose predecessor is also permanently gated off |
