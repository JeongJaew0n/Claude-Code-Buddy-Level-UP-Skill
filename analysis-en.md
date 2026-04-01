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

## 13. Changing Buddy Language via Personality

By modifying `companion.personality`, you can also **change the language** your buddy speaks in.

### Verified Result

Including a language instruction in the personality value makes the buddy speak in that language:

```json
{
  "companion": {
    "personality": "한국어로만 말하는 겁 많은 나방. 터미널의 따뜻한 빛에 이끌려 온 나방으로, 디버깅 세션을 자연 다큐멘터리처럼 해설하고 중첩 삼항연산자를 보면 깜짝 놀란다."
  }
}
```

### Multi-language Examples

| Language | Personality Example |
|----------|-------------------|
| Korean | `"한국어로만 말하는 호기심 많은 토끼"` |
| Japanese | `"日本語だけで話す好奇心旺盛なうさぎ"` |
| Chinese | `"只用中文说话的好奇兔子"` |
| Spanish | `"Un conejo curioso que solo habla en español"` |
| French | `"Un lapin curieux qui ne parle qu'en français"` |

**Key insight: The personality field is not just a character description — it acts as a prompt that controls the buddy's behavior.** Including language instructions will make the buddy speak in that language.

## 14. Mismatch Between Species Field and Actual Rendering

The `companion.species` value stored in `~/.claude.json` and the actual buddy rendered on screen **can be different.**

### Verified Result

- `~/.claude.json` species value: `cat`
- Actual rendering: rabbit

### Cause

The buddy's actual rendering does not reference the species field in `~/.claude.json`. Instead, it **runs the account UUID-based PRNG every time** to determine the species. This means:

1. The species value in `~/.claude.json` may be from initial generation or manually modified
2. The actual rendering always uses the PRNG result seeded by `oauthAccount.accountUuid`
3. Manually changing the species field has **no effect on rendering**

**Key insight: The species field in `~/.claude.json` is unreliable.** The actual buddy appearance is determined at runtime by the account UUID.
