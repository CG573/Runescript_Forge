# ⚒ Runic Script Forge

A block-based notation tool for composing runic enchantment scripts. Built for artificers working with the Vampyric Rifle rune system.

## How to Use

1. **Select a clause** in the Script Builder (or start with Clause 01 already active)
2. **Click runes** in the left-hand Lexicon panel to add them to the clause
3. **Add clauses** with the "+ Add Clause" button — clauses are separated by `‖`
4. The **Script Output** updates in real time, showing both the rune string and a plain-English translation
5. Use **Copy Script** to grab the rune characters, or **Copy Translation** for the human-readable version

## Rune Syntax

Scripts follow this pattern per clause:

```
[CONDITIONAL] → [NOUN] [VERB] [NOUN] [MODIFIER?]
```

- **Conditionals** (red) go first — they set the gate condition. An `→` is inserted automatically after them.
- **Nouns** (blue) are the subjects — life force, the wielder, the target, etc.
- **Verbs** (amber) are the operations — draw, transfer, read, bind, amplify, suppress.
- **Modifiers** (green) scale or alter the verb's output.

Multiple clauses chain together. Use the `Ing` conditional to make a clause fire only if the previous one succeeded — this is your null-check / chained gate.
