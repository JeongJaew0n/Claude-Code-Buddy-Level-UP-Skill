# Claude-Code-Buddy-Rename-Skill

A Claude Code skill to rename your `/buddy` companion.

## What is /buddy?

`/buddy` is a Claude Code feature (available since April 2026) that gives you a virtual companion displayed next to your input box. Each buddy has a unique species, eyes, hat, rarity, and stats — all deterministically generated from your account UUID.

**The only things you can change are the name and personality.** This skill makes renaming easy.

## Installation

Copy the `buddy-rename/` folder into your Claude Code skills directory:

```bash
cp -r buddy-rename/ ~/.claude/skills/buddy-rename/
```

## Usage

Invoke the skill in Claude Code:

```
/buddy-rename NewName
```

Or just say:
- "rename my buddy"
- "change buddy name to Mochi"
- "buddy 이름 바꿔줘"

If no name is provided, you'll be prompted to enter one.

### Constraints

- Name must be **14 characters or less**
- Restart Claude Code after renaming for the change to take effect

## How it works

The skill edits `~/.claude.json` > `companion.name`. That's it.

```json
{
  "companion": {
    "name": "YourNewName",
    "species": "rabbit",
    "eye": "dot",
    "hat": "none",
    ...
  }
}
```

## What you CAN'T change

| Property | Why |
|----------|-----|
| species | Deterministic PRNG seeded by account UUID |
| eye | Same PRNG, fixed to account |
| hat | Same PRNG, fixed to account |
| rarity | Same PRNG, fixed to account |
| stats | Same PRNG, fixed to account |

See [analysis-en.md](analysis-en.md) for the full reverse-engineering analysis, or [analysis-ko.md](analysis-ko.md) for the Korean version.

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
<summary>한국어</summary>

# Claude-Code-Buddy-Rename-Skill

Claude Code의 `/buddy` 컴패니언 이름을 변경하는 스킬입니다.

## /buddy란?

`/buddy`는 Claude Code의 기능으로 (2026년 4월부터 사용 가능), 입력창 옆에 표시되는 가상 컴패니언입니다. 각 buddy는 고유한 species, eye, hat, rarity, stats를 가지며, 모두 계정 UUID를 기반으로 결정론적으로 생성됩니다.

**변경할 수 있는 것은 이름과 성격뿐입니다.** 이 스킬은 이름 변경을 간편하게 해줍니다.

## 설치

`buddy-rename/` 폴더를 Claude Code 스킬 디렉토리에 복사하세요:

```bash
cp -r buddy-rename/ ~/.claude/skills/buddy-rename/
```

## 사용법

Claude Code에서 스킬을 호출하세요:

```
/buddy-rename 새이름
```

또는 자연어로:
- "buddy 이름 바꿔줘"
- "버디 이름을 모찌로 변경해줘"
- "rename my buddy"

이름을 입력하지 않으면 입력을 요청합니다.

### 제약 사항

- 이름은 **최대 14자**
- 이름 변경 후 Claude Code를 재시작해야 적용됩니다

## 작동 방식

`~/.claude.json` > `companion.name`을 수정합니다. 그게 전부입니다.

```json
{
  "companion": {
    "name": "새이름",
    "species": "rabbit",
    "eye": "dot",
    "hat": "none",
    ...
  }
}
```

## 변경할 수 없는 것

| 속성 | 이유 |
|------|------|
| species | 계정 UUID 기반 결정론적 PRNG |
| eye | 동일 PRNG, 계정에 고정 |
| hat | 동일 PRNG, 계정에 고정 |
| rarity | 동일 PRNG, 계정에 고정 |
| stats | 동일 PRNG, 계정에 고정 |

상세 분석은 [analysis-ko.md](analysis-ko.md) (한국어) 또는 [analysis-en.md](analysis-en.md) (영어)를 참고하세요.

</details>

<details>
<summary>中文</summary>

# Claude-Code-Buddy-Rename-Skill

用于重命名 Claude Code `/buddy` 伙伴的技能。

## 什么是 /buddy？

`/buddy` 是 Claude Code 的功能（2026年4月起可用），在输入框旁边显示一个虚拟伙伴。每个 buddy 都有独特的 species、eye、hat、rarity 和 stats——全部基于账户 UUID 确定性生成。

**你唯一能改变的是名字和性格。** 这个技能让重命名变得简单。

## 安装

将 `buddy-rename/` 文件夹复制到 Claude Code 技能目录：

```bash
cp -r buddy-rename/ ~/.claude/skills/buddy-rename/
```

## 使用方法

在 Claude Code 中调用技能：

```
/buddy-rename 新名字
```

或者用自然语言：
- "rename my buddy"
- "把 buddy 的名字改成 Mochi"
- "给伙伴改名"

如果未提供名字，系统会提示你输入。

### 限制

- 名字最长 **14个字符**
- 重命名后需要重启 Claude Code 才能生效

## 工作原理

编辑 `~/.claude.json` > `companion.name`。就这么简单。

```json
{
  "companion": {
    "name": "新名字",
    "species": "rabbit",
    "eye": "dot",
    "hat": "none",
    ...
  }
}
```

## 无法更改的内容

| 属性 | 原因 |
|------|------|
| species | 基于账户 UUID 的确定性 PRNG |
| eye | 相同 PRNG，绑定账户 |
| hat | 相同 PRNG，绑定账户 |
| rarity | 相同 PRNG，绑定账户 |
| stats | 相同 PRNG，绑定账户 |

详细分析请参阅 [analysis-en.md](analysis-en.md)（英文）或 [analysis-ko.md](analysis-ko.md)（韩文）。

</details>

<details>
<summary>日本語</summary>

# Claude-Code-Buddy-Rename-Skill

Claude Code の `/buddy` コンパニオンの名前を変更するスキルです。

## /buddy とは？

`/buddy` は Claude Code の機能で（2026年4月から利用可能）、入力ボックスの横にバーチャルコンパニオンを表示します。各 buddy は固有の species、eye、hat、rarity、stats を持ち、すべてアカウント UUID に基づいて決定論的に生成されます。

**変更できるのは名前とパーソナリティだけです。** このスキルで簡単に名前を変更できます。

## インストール

`buddy-rename/` フォルダを Claude Code のスキルディレクトリにコピーしてください：

```bash
cp -r buddy-rename/ ~/.claude/skills/buddy-rename/
```

## 使い方

Claude Code でスキルを呼び出します：

```
/buddy-rename 新しい名前
```

または自然言語で：
- "rename my buddy"
- "buddy の名前を Mochi に変えて"
- "バディの名前を変更して"

名前が指定されていない場合、入力を求められます。

### 制約事項

- 名前は**最大14文字**
- 名前変更後、Claude Code を再起動すると反映されます

## 仕組み

`~/.claude.json` > `companion.name` を編集します。それだけです。

```json
{
  "companion": {
    "name": "新しい名前",
    "species": "rabbit",
    "eye": "dot",
    "hat": "none",
    ...
  }
}
```

## 変更できないもの

| プロパティ | 理由 |
|-----------|------|
| species | アカウント UUID ベースの決定論的 PRNG |
| eye | 同じ PRNG、アカウントに固定 |
| hat | 同じ PRNG、アカウントに固定 |
| rarity | 同じ PRNG、アカウントに固定 |
| stats | 同じ PRNG、アカウントに固定 |

詳細な分析は [analysis-en.md](analysis-en.md)（英語）または [analysis-ko.md](analysis-ko.md)（韓国語）をご覧ください。

</details>
