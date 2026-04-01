# Claude-Code-Buddy-Level-UP-Skill

> **Language**: [English](#claude-code-buddy-level-up-skill) | [한국어](#한국어) | [中文](#中文) | [日本語](#日本語)

A Claude Code skill and analysis toolkit for customizing your `/buddy` companion.

## Features

- **Rename Buddy** — Change your buddy's name (max 14 characters)
- **Change Language** — Make your buddy speak in any language via personality field
- **Customize Personality** — Define your buddy's character, tone, and behavior
- **Deep Analysis** — Full reverse-engineering analysis of the /buddy feature (15 sections)
  - Species generation mechanism (deterministic PRNG)
  - Rarity, stats, eye, hat system
  - Speech trigger conditions (test failures, build errors)
  - Rendering and activation internals

## What You Can Change

| Property | How |
|----------|-----|
| **Name** | `/buddy-rename NewName` or edit `~/.claude.json` |
| **Personality** | Edit `companion.personality` in `~/.claude.json` |
| **Language** | Include language instruction in personality (e.g. `"한국어로만 말하는 토끼"`) |

## What You CAN'T Change

| Property | Why |
|----------|-----|
| species | Deterministic PRNG seeded by account UUID |
| eye | Same PRNG, fixed to account |
| hat | Same PRNG, fixed to account |
| rarity | Same PRNG, fixed to account |
| stats | Same PRNG, fixed to account |

## Installation

Copy the `buddy-rename/` folder into your Claude Code skills directory:

```bash
cp -r buddy-rename/ ~/.claude/skills/buddy-rename/
```

## Usage

**Rename:**
```
/buddy-rename NewName
```
Or just say: "rename my buddy", "buddy 이름 바꿔줘"

**Change language** (edit `~/.claude.json` manually):
```json
{ "companion": { "personality": "한국어로만 말하는 소심한 토끼" } }
```

Restart Claude Code after any change to apply.

## Analysis

Full reverse-engineering analysis of the /buddy feature is available:
- [analysis-en.md](analysis-en.md) (English)
- [analysis-ko.md](analysis-ko.md) (Korean)

Covers: species generation, PRNG internals, rarity system, rendering functions, speech triggers, activation conditions, and more.

## Project Structure

```
.
├── README.md
├── analysis-en.md        # /buddy feature analysis (English)
├── analysis-ko.md        # /buddy feature analysis (Korean)
└── buddy-rename/
    └── SKILL.md           # The skill definition
```

## License

MIT

---

<details>
<summary>

## 한국어

</summary>

# Claude-Code-Buddy-Level-UP-Skill

Claude Code의 `/buddy` 컴패니언을 커스터마이징하는 스킬 및 분석 툴킷입니다.

## 기능

- **이름 변경** — buddy 이름 변경 (최대 14자)
- **언어 변경** — personality 필드를 통해 buddy가 원하는 언어로 말하게 하기
- **성격 커스터마이징** — buddy의 캐릭터, 말투, 행동 정의
- **심층 분석** — /buddy 기능 리버스 엔지니어링 분석 (15개 섹션)

## 변경 가능한 것

| 속성 | 방법 |
|------|------|
| **이름** | `/buddy-rename 새이름` 또는 `~/.claude.json` 편집 |
| **성격** | `~/.claude.json`의 `companion.personality` 편집 |
| **언어** | personality에 언어 지시 포함 (예: `"한국어로만 말하는 토끼"`) |

## 변경할 수 없는 것

| 속성 | 이유 |
|------|------|
| species | 계정 UUID 기반 결정론적 PRNG |
| eye | 동일 PRNG, 계정에 고정 |
| hat | 동일 PRNG, 계정에 고정 |
| rarity | 동일 PRNG, 계정에 고정 |
| stats | 동일 PRNG, 계정에 고정 |

## 설치

```bash
cp -r buddy-rename/ ~/.claude/skills/buddy-rename/
```

## 사용법

**이름 변경:**
```
/buddy-rename 새이름
```
또는: "buddy 이름 바꿔줘", "rename my buddy"

**언어 변경** (`~/.claude.json` 직접 편집):
```json
{ "companion": { "personality": "한국어로만 말하는 소심한 토끼" } }
```

변경 후 Claude Code를 재시작하면 적용됩니다.

## 분석

상세 분석은 [analysis-ko.md](analysis-ko.md) (한국어) 또는 [analysis-en.md](analysis-en.md) (영어)를 참고하세요.

</details>

<details>
<summary>

## 中文

</summary>

# Claude-Code-Buddy-Level-UP-Skill

用于自定义 Claude Code `/buddy` 伙伴的技能和分析工具包。

## 功能

- **重命名 Buddy** — 更改 buddy 名字（最多14个字符）
- **更改语言** — 通过 personality 字段让 buddy 使用任意语言
- **自定义性格** — 定义 buddy 的角色、语气和行为
- **深度分析** — /buddy 功能逆向工程分析（15个章节）

## 可以更改的内容

| 属性 | 方法 |
|------|------|
| **名字** | `/buddy-rename 新名字` 或编辑 `~/.claude.json` |
| **性格** | 编辑 `~/.claude.json` 中的 `companion.personality` |
| **语言** | 在 personality 中包含语言指令（如 `"只用中文说话的好奇兔子"`） |

## 无法更改的内容

| 属性 | 原因 |
|------|------|
| species | 基于账户 UUID 的确定性 PRNG |
| eye | 相同 PRNG，绑定账户 |
| hat | 相同 PRNG，绑定账户 |
| rarity | 相同 PRNG，绑定账户 |
| stats | 相同 PRNG，绑定账户 |

## 安装

```bash
cp -r buddy-rename/ ~/.claude/skills/buddy-rename/
```

## 使用方法

**重命名：**
```
/buddy-rename 新名字
```
或者："rename my buddy"、"给伙伴改名"

**更改语言**（直接编辑 `~/.claude.json`）：
```json
{ "companion": { "personality": "只用中文说话的好奇兔子" } }
```

更改后重启 Claude Code 即可生效。

## 分析

详细分析请参阅 [analysis-en.md](analysis-en.md)（英文）或 [analysis-ko.md](analysis-ko.md)（韩文）。

</details>

<details>
<summary>

## 日本語

</summary>

# Claude-Code-Buddy-Level-UP-Skill

Claude Code の `/buddy` コンパニオンをカスタマイズするスキルと分析ツールキットです。

## 機能

- **名前変更** — buddy の名前を変更（最大14文字）
- **言語変更** — personality フィールドで buddy が好きな言語で話すように設定
- **パーソナリティのカスタマイズ** — buddy のキャラクター、口調、行動を定義
- **詳細分析** — /buddy 機能のリバースエンジニアリング分析（15セクション）

## 変更できるもの

| プロパティ | 方法 |
|-----------|------|
| **名前** | `/buddy-rename 新しい名前` または `~/.claude.json` を編集 |
| **パーソナリティ** | `~/.claude.json` の `companion.personality` を編集 |
| **言語** | personality に言語指示を含める（例：`"日本語だけで話す好奇心旺盛なうさぎ"`） |

## 変更できないもの

| プロパティ | 理由 |
|-----------|------|
| species | アカウント UUID ベースの決定論的 PRNG |
| eye | 同じ PRNG、アカウントに固定 |
| hat | 同じ PRNG、アカウントに固定 |
| rarity | 同じ PRNG、アカウントに固定 |
| stats | 同じ PRNG、アカウントに固定 |

## インストール

```bash
cp -r buddy-rename/ ~/.claude/skills/buddy-rename/
```

## 使い方

**名前変更：**
```
/buddy-rename 新しい名前
```
または："buddy の名前を変えて"、"rename my buddy"

**言語変更**（`~/.claude.json` を直接編集）：
```json
{ "companion": { "personality": "日本語だけで話す好奇心旺盛なうさぎ" } }
```

変更後、Claude Code を再起動すると反映されます。

## 分析

詳細な分析は [analysis-en.md](analysis-en.md)（英語）または [analysis-ko.md](analysis-ko.md)（韓国語）をご覧ください。

</details>
