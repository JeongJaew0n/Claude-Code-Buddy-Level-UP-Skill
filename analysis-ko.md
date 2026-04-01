# Claude Code /buddy 기능 분석

## 1. 데이터 저장 위치

- `~/.claude.json`의 `companion` 필드에 저장
- Claude Code 시작 시 이 파일을 읽어 buddy 상태를 로드

## 2. companion 객체 구조

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

## 3. Species 생성 메커니즘

species(모양)는 계정 UUID를 시드로 한 **deterministic PRNG**로 생성된다.

### 함수 호출 체인

```
hN6() → aN4() → oN4() → vN6()
```

- `hN6()`: 최상위 생성 함수. companion 전체 속성을 결정
- `aN4()`: PRNG 시드 초기화
- `oN4()`: 시드 기반 난수 생성기
- `vN6()`: 가중치 배열에서 확률적 선택

### 시드 소스

```javascript
oauthAccount.accountUuid ?? userID ?? 'anon'
```

- OAuth 계정의 UUID가 최우선
- 없으면 userID 사용
- 둘 다 없으면 `'anon'` 문자열 사용

**핵심: 동일 계정이면 항상 동일한 species가 생성된다.**

## 4. Species 종류 (18개)

| # | Species | 설명 |
|---|---------|------|
| 1 | fish | 물고기 |
| 2 | bird | 새 |
| 3 | frog | 개구리 |
| 4 | cat | 고양이 |
| 5 | bat | 박쥐 |
| 6 | jellyfish | 해파리 |
| 7 | owl | 부엉이 |
| 8 | snail | 달팽이 |
| 9 | rabbit | 토끼 |
| 10 | mouse | 쥐 |
| 11 | blob | 블롭 |
| 12 | bug | 벌레 |
| 13 | fox | 여우 |
| 14 | bear | 곰 |
| 15 | moth | 나방 |
| 16 | crab | 게 |
| 17 | turtle | 거북이 |
| 18 | squid | 오징어 |

## 5. Eye 종류 (6개)

| Eye | 문자 | 설명 |
|-----|------|------|
| dot | `·` | 중점 |
| star | `✦` | 사각별 |
| cross | `×` | 곱셈 |
| circle | `◎` | 이중원 |
| at | `@` | 골뱅이 |
| degree | `°` | 도 |

## 6. Hat 종류 (8개)

| # | Hat | 설명 |
|---|-----|------|
| 1 | none | 없음 |
| 2 | crown | 왕관 |
| 3 | tophat | 실크햇 |
| 4 | propeller | 프로펠러 |
| 5 | halo | 천사 고리 |
| 6 | wizard | 마법사 모자 |
| 7 | beanie | 비니 |
| 8 | tinyduck | 작은 오리 |

## 7. Rarity (희귀도)

가중치 기반 확률 분포:

| Rarity | 가중치 | 확률 |
|--------|--------|------|
| common | 60 | 60% |
| uncommon | 25 | 25% |
| rare | 10 | 10% |
| epic | 4 | 4% |
| legendary | 1 | 1% |

## 8. Stats (5개)

각 stat은 PRNG로 결정되며, buddy의 "성격 수치"를 나타낸다.

| Stat | 설명 |
|------|------|
| DEBUGGING | 디버깅 능력 |
| PATIENCE | 인내심 |
| CHAOS | 혼돈 성향 |
| WISDOM | 지혜 |
| SNARK | 비꼼/재치 |

## 9. 이름 생성

### 기본 이름 후보 (6개)

```
Crumpet, Soup, Pickle, Biscuit, Moth, Gravy
```

### 수식어 단어 (130개+)

이름 앞에 붙는 수식어로, 조합하여 유니크한 이름을 생성한다.

예시:
```
thunder, biscuit, void, accordion, moss, velvet, rust, pickle,
phantom, waffle, cosmic, noodle, shadow, glitter, turbo, ...
```

최종 이름 형식: `{수식어}{기본이름}` (예: `ThunderPickle`, `VoidMoth`)

## 10. 렌더링

### ASCII 아트 매핑

```javascript
li_(H, _ = 0)
```

- `sk7[H.species]`에서 species별 ASCII 아트 템플릿을 조회
- eye, hat 등의 속성을 템플릿에 적용하여 최종 렌더링
- `_` 파라미터는 애니메이션 프레임 인덱스

## 11. /buddy 활성화 조건

```javascript
LK8()
```

- **firstParty**: Anthropic 공식 클라이언트(Claude Code)에서만 활성화
- **날짜 조건**: 2026년 4월 이후에만 표시
- 서드파티 클라이언트나 이전 날짜에서는 buddy가 나타나지 않음

## 12. 핵심 발견 요약

| 항목 | 변경 가능 여부 | 방법 |
|------|---------------|------|
| name | ✅ 가능 | `~/.claude.json`의 `companion.name` 수정 |
| personality | ✅ 가능 | `~/.claude.json`의 `companion.personality` 수정 |
| species | ❌ 불가 | 계정 UUID 기반 deterministic 생성, 1:1 고정 |
| eye | ❌ 불가 | PRNG로 결정, 계정에 고정 |
| hat | ❌ 불가 | PRNG로 결정, 계정에 고정 |
| rarity | ❌ 불가 | PRNG로 결정, 계정에 고정 |
| stats | ❌ 불가 | PRNG로 결정, 계정에 고정 |

**species는 계정에 1:1로 고정되므로 변경이 불가능하다.** `~/.claude.json`에서 species 값을 직접 수정하더라도, Claude Code 재시작 시 계정 UUID 기반으로 다시 생성되어 원래 값으로 돌아간다.

사용자가 변경할 수 있는 것은 **이름(name)**과 **성격(personality)**뿐이며, 이는 `~/.claude.json`을 직접 편집하거나 `/buddy-rename` 스킬을 통해 변경할 수 있다.

## 13. Personality를 통한 언어 변경

`companion.personality` 필드를 수정하면 buddy가 사용하는 **언어도 변경할 수 있다.**

### 검증 결과

personality 값에 언어 지시를 포함하면 buddy가 해당 언어로 말한다:

```json
{
  "companion": {
    "personality": "한국어로만 말하는 겁 많은 나방. 터미널의 따뜻한 빛에 이끌려 온 나방으로, 디버깅 세션을 자연 다큐멘터리처럼 해설하고 중첩 삼항연산자를 보면 깜짝 놀란다."
  }
}
```

### 다국어 예시

| 언어 | personality 예시 |
|------|-----------------|
| 한국어 | `"한국어로만 말하는 호기심 많은 토끼"` |
| 日本語 | `"日本語だけで話す好奇心旺盛なうさぎ"` |
| 中文 | `"只用中文说话的好奇兔子"` |
| Español | `"Un conejo curioso que solo habla en español"` |
| Français | `"Un lapin curieux qui ne parle qu'en français"` |

**핵심: personality 필드는 단순한 성격 설명이 아니라, buddy의 행동을 제어하는 프롬프트 역할을 한다.** 언어 지시를 포함하면 해당 언어로 말하게 할 수 있다.

## 14. Species 필드와 실제 렌더링의 불일치

`~/.claude.json`에 저장된 `companion.species` 값과 실제 화면에 렌더링되는 buddy 모양이 **다를 수 있다.**

### 검증 결과

- `~/.claude.json`의 species 값: `cat`
- 실제 렌더링: 토끼 (rabbit)

### 원인

buddy의 실제 렌더링은 `~/.claude.json`의 species 필드를 참조하지 않고, **계정 UUID 기반 PRNG를 매번 실행하여 결정**하는 것으로 추정된다. 즉:

1. `~/.claude.json`의 species 값은 최초 생성 시 기록된 값이거나, 사용자가 임의로 수정한 값일 수 있음
2. 실제 렌더링은 항상 `oauthAccount.accountUuid`를 시드로 한 PRNG 결과를 사용
3. 따라서 species 필드를 수동으로 변경해도 **렌더링에는 영향 없음**

**핵심: `~/.claude.json`의 species 필드는 신뢰할 수 없다.** 실제 buddy 모양은 계정 UUID에 의해 런타임에 결정된다.
