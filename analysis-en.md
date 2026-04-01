# Claude Code /buddy Feature Analysis

## 1. Data Storage Location

- Stored in the `companion` field of `~/.claude.json`
- Claude Code reads this file on startup to load buddy state

## 2. companion Object Structure

```json
{
  "companion": {
    "name": "래뷧",
    "personality": "curious",
    "hatchedAt": "2026-04-01T00:00:00.000Z",
    "species": "rabbit",
    "eye": "dot",
    "hat": "none",
    "shiny": false,
    "rarity": "common",
    "stats": {
      "DEBUGGING": 7,
      "PATIENCE": 5,
      "CHAOS": 3,
      "WISDOM": 8,
      "SNARK": 4
    }
  }
}
```

## 3. Species Generation Mechanism

Species (shape) is generated via a **deterministic PRNG** seeded with the account UUID.

### Function Call Chain

```
hN6() → aN4() → oN4() → vN6()
```

- `hN6()`: Top-level generation function. Determines all companion attributes
- `aN4()`: PRNG seed initialization
- `oN4()`: Seed-based random number generator
- `vN6()`: Probabilistic selection from weighted arrays

### Seed Source

```javascript
oauthAccount.accountUuid ?? userID ?? 'anon'
```

- OAuth account UUID takes highest priority
- Falls back to userID if unavailable
- Uses the string `'anon'` if neither is available

**Key insight: The same account always generates the same species.**

## 4. Species Types (18)

| # | Species | Description |
|---|---------|-------------|
| 1 | fish | Fish |
| 2 | bird | Bird |
| 3 | frog | Frog |
| 4 | cat | Cat |
| 5 | bat | Bat |
| 6 | jellyfish | Jellyfish |
| 7 | owl | Owl |
| 8 | snail | Snail |
| 9 | rabbit | Rabbit |
| 10 | mouse | Mouse |
| 11 | blob | Blob |
| 12 | bug | Bug |
| 13 | fox | Fox |
| 14 | bear | Bear |
| 15 | moth | Moth |
| 16 | crab | Crab |
| 17 | turtle | Turtle |
| 18 | squid | Squid |

## 5. Eye Types (6)

| Eye | Character | Description |
|-----|-----------|-------------|
| dot | `·` | Middle dot |
| star | `✦` | Four-pointed star |
| cross | `×` | Multiplication sign |
| circle | `◎` | Double circle |
| at | `@` | At sign |
| degree | `°` | Degree sign |

## 6. Hat Types (8)

| # | Hat | Description |
|---|-----|-------------|
| 1 | none | No hat |
| 2 | crown | Crown |
| 3 | tophat | Top hat |
| 4 | propeller | Propeller hat |
| 5 | halo | Angel halo |
| 6 | wizard | Wizard hat |
| 7 | beanie | Beanie |
| 8 | tinyduck | Tiny duck |

## 7. Rarity

Weight-based probability distribution:

| Rarity | Weight | Probability |
|--------|--------|-------------|
| common | 60 | 60% |
| uncommon | 25 | 25% |
| rare | 10 | 10% |
| epic | 4 | 4% |
| legendary | 1 | 1% |

## 8. Stats (5)

Each stat is determined by the PRNG and represents the buddy's "personality scores."

| Stat | Description |
|------|-------------|
| DEBUGGING | Debugging ability |
| PATIENCE | Patience level |
| CHAOS | Chaos tendency |
| WISDOM | Wisdom |
| SNARK | Snark / wit |

## 9. Name Generation

### Base Name Candidates (6)

```
Crumpet, Soup, Pickle, Biscuit, Moth, Gravy
```

### Modifier Words (130+)

Prefixed to base names to create unique combinations.

Examples:
```
thunder, biscuit, void, accordion, moss, velvet, rust, pickle,
phantom, waffle, cosmic, noodle, shadow, glitter, turbo, ...
```

Final name format: `{modifier}{baseName}` (e.g., `ThunderPickle`, `VoidMoth`)

## 10. Rendering

### ASCII Art Mapping

```javascript
li_(H, _ = 0)
```

- Looks up species-specific ASCII art templates from `sk7[H.species]`
- Applies eye, hat, and other attributes to the template for final rendering
- The `_` parameter is the animation frame index

## 11. /buddy Activation Conditions

```javascript
LK8()
```

- **firstParty**: Only activates in Anthropic's official client (Claude Code)
- **Date condition**: Only displayed after April 2026
- Buddy does not appear in third-party clients or before the activation date

## 12. Key Findings Summary

| Property | Modifiable | Method |
|----------|-----------|--------|
| name | ✅ Yes | Edit `companion.name` in `~/.claude.json` |
| personality | ✅ Yes | Edit `companion.personality` in `~/.claude.json` |
| species | ❌ No | Deterministic generation based on account UUID, fixed 1:1 |
| eye | ❌ No | Determined by PRNG, fixed to account |
| hat | ❌ No | Determined by PRNG, fixed to account |
| rarity | ❌ No | Determined by PRNG, fixed to account |
| stats | ❌ No | Determined by PRNG, fixed to account |

**Species is fixed 1:1 to the account and cannot be changed.** Even if you manually edit the species value in `~/.claude.json`, Claude Code will regenerate it based on the account UUID on restart, reverting it to the original value.

The only properties users can change are **name** and **personality**, either by directly editing `~/.claude.json` or by using the `/buddy-rename` skill.
