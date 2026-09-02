# 영어 분경조 문법

Veo·Sora·Runway 계열에 넣는 형태다.

중국어판이 `【】` 라벨에 기대는 것과 달리, 영어권 모델은 **대문자 섹션 헤더 + 짧은 문장**을 더 잘 받는다. 의미 구조는 같고 표기만 다르다. 라벨을 한자로 남겨두면 모델이 그걸 화면 텍스트로 렌더하는 사고가 난다.

아래는 전부 **슬롯 템플릿**이다. `<>` 안은 이번 작품의 값으로 채운다.
슬롯을 참조 자료에 있던 값으로 채우면 남의 작품이 튀어나온다.

## 목차

1. 블록 순서
2. 고정층 템플릿
3. 변동층 템플릿
4. 중국어판과 다르게 가는 지점
5. 표현 대응표

---

## 1. 블록 순서

```
STYLE                 스타일·질감          고정
LIGHTING              광영 설계            고정
REFERENCE BINDING     소재 바인딩          고정 (참조 이미지 사용 시)
SPACE LOCK            배경 구역            고정
DEPTH TIERS           심도 등급            고정
VOICE PROFILE         캐릭터 음색표        고정
SHOT GROUP N — <부제> — <초>s              변동
SCENE                 씬 헤더              변동
CAST / PROPS          등장 목록            변동
FRAMING               배경 취경            변동
SHOT 1 — <초>s — <화각>/<앵글> (<상태>)     변동
  action
  VOICE: …
  Show only what is described in this shot.
SHOT 2 …
END STATE             종료 상태            변동
HANDOFF → S<NN>       인계                 변동
CORE GUIDELINES       최우선 접미          고정 — 맨 뒤
```

---

## 2. 고정층 템플릿

### STYLE

```
STYLE — <화면비>. <장르 질감>. <렌더 스타일>. <화질 기준 3~5개>.
No subtitles. No watermark. No background music. Clean frame.
<반드시 선명해야 할 것>.
This episode's primary effect: <주 이펙트>, tied to the prop and its motion.
```

- `<화질 기준>` — 3~5개만 고른다. 더 쌓아도 효과가 안 늘고 슬롭 쪽으로 민다
- `<반드시 선명해야 할 것>` — 흐려지면 안 되는 것. 대화극이면 표정과 손, 제품이면 로고와 재질
- `This episode's primary effect` — 회차마다 바뀌는 유일한 칸

금지 세 줄은 슬롯이 아니다. 빼면 자막이 그려지고 스톡 음악이 깔린다.

### LIGHTING

```
LIGHTING — <색온도>K <색 성격>. Key at <각도>° camera-side, <각도>° rim backlight,
<앰비언트 강도> ambient fill. Global shadow depth <값>, soft edge <값>, falloff <값>.
Catchlight in the eyes.
Specular weighting: <주인공 재질> ×<배수>, <중간 재질> ×<배수>, <바닥 재질> ×<배수>.
Hold normal overall exposure, even facial light, a single light direction,
natural source colour.
```

`Specular weighting` 줄이 핵심이다. 영어권 모델도 상대비를 잘 받는다 — 오히려 `×1.5` 같은 배수 표기를 중국어판보다 안정적으로 읽는다. 어떤 재질을 주인공으로 삼을지는 `lighting-materials.md`.

마지막 두 줄 네 조항은 슬롯이 아니다. `a single light direction` 이 빠지면 컷마다 광원이 옮겨 다닌다.

### REFERENCE BINDING

```
REFERENCE BINDING — <N> reference images.
@<캐릭터> = image N = <파일명>. Sole identity reference for <캐릭터>: lock the same face,
age, <헤어>, <체형>, plus <의상> and <신발>.
@<배경인물> = image N = <파일명>. Locks period wardrobe only. Does not affect the faces
of <주요 인물들>.
@tags are asset handles. Never render them as on-screen text.
```

마지막 줄은 반드시 넣는다. 빼면 `@` 태그가 화면에 글자로 나온다.

### CORE GUIDELINES

맨 뒤. 담아야 하는 것은 네 가지고, 문장은 이 뜻이 전달되는 선에서 직접 쓴다.

| 조항 | 없으면 |
|---|---|
| 프롬프트를 이탈 없이 따를 것 | 모델이 "더 좋게" 각색한다 |
| BGM 금지, 앰비언트와 생목소리만 | 스톡 배경음악이 깔린다 |
| 대사는 제공된 문구와 정확히 일치, `<언어>`로 완전히 발화 | 말을 줄이거나 지어낸다 |
| 자막 금지 | 화면에 글자가 박힌다 |

```
CORE GUIDELINES — <위 네 조항. 대사 언어를 명시한다.>
```

대사 언어를 안 적으면 모델이 프롬프트 언어를 따라간다. 영어 프롬프트에 한국어 대사를 넣을 때 특히 위험하다.

---

## 3. 변동층 템플릿

### SCENE

```
SCENE — <장소> [@ref N], <실외/실내>, <시간>.
Foreground: <인물A> [@ref 1] <상태와 위치>.
Mid-ground: <인물B> [@ref 2] <상태>. <인물C> [@ref 3] <상태>.
<광원 상태>.
```

### CAST / PROPS

```
CAST — <인물A> [@ref 1], <인물B> [@ref 2]
BACKGROUND (low detail, non-performing) — <인물C> [@ref 3], <위치>
PROPS — <소도구>
Do not introduce any character, alias, location or prop not listed here.
```

`low detail, non-performing` 표기가 중요하다. 없으면 배경 인물이 주연급 디테일을 먹고 리롤 확률이 오른다.

### SHOT

```
SHOT N — <초>s — <close/medium/wide>/<eye-level/low angle> (<상태>)
<인물> [@ref N] <액션 1~2줄. 관찰 가능한 동작만>
VOICE: <인물> [@ref N] | state: <상태> | delivery: <톤/속도/문말>
  "<대사 원문>"
FRAMING: <들어오는 구역과 위치>. <안 들어오는 것> out of frame. Background <blur> (f/<값>).
Show only what is described in this shot. Add no unlisted person, object, effect or
background element.
```

대사 없는 컷은 `VOICE: none`.

`FRAMING` 은 배경이 있는 모든 컷에 붙인다. 없으면 근경에서 모델이 배경 전체를 우겨넣으려 화각을 뒤로 뺀다.
**안 들어오는 것을 명시적으로 적는다.** 설계법은 `scene-space.md`.

발화 싱크:

```
On "<단어1>" <동작1>. On "<단어2>" <동작2>. On "<단어3>" <동작3>.
```

영어권 모델은 이 패턴을 잘 받는다. 따옴표 안 단어를 대사 원문과 **글자 그대로** 일치시킨다.

### END STATE / HANDOFF

```
END STATE — <인물A> [@ref 1]: <상태>. <인물B> [@ref 2]: <상태>. <인물C> [@ref 3]: <상태>.

HANDOFF → S<다음화번호> — <렌즈>mm reaction close-up on <인물>, <소도구 위치>,
<표정>, eyes on <대상>, lips about to part.
S<다음화번호> opens on <인물>'s reply: "<다음 화 첫 대사>"
```

---

## 4. 중국어판과 다르게 가는 지점

| | 중국어 | 영어 |
|---|---|---|
| 라벨 | `【】` 한자 | 대문자 섹션 헤더 |
| 문장 | 세미콜론으로 길게 이어붙임 | **짧은 문장으로 끊는다.** 길게 이으면 뒷부분이 먹힌다 |
| 렌즈 | `50mm中近景` | `50mm medium close-up` — 초점거리를 더 잘 받는다 |
| 부정 지시 | `禁止…` 한 줄 | `Do not…` + 컷마다 반복. 한 번만 쓰면 약하다 |
| 시간 표기 | `0—2秒` | `0:00–0:02` 또는 `first 2 seconds` |
| 연속성 | `【人物与状态连续性】` 블록 | `CONTINUITY` 섹션 — 아래 |

```
CONTINUITY — <인물A> reads image 1 only. <인물B> reads image 2 only.
<인물A> keeps <고정 의상> and does not shift to <흔한 오답>.
<인물B>'s <헤어>, <의상>, <신발> and <소도구> stay identical throughout.
Faces, age, skin tone, hair and wardrobe hold through head turns, speech and hard cuts.
<환경 연속 요소> stays continuous.
```

`does not shift to <흔한 오답>` — 틀리는 방향을 직접 지목하는 게 실전에서 잘 듣는다. 몇 번 뽑아보면 모델이 늘 같은 쪽으로 틀린다(특정 색, 소매 길이, 사라지는 액세서리). 그걸 알아낸 뒤 채운다. 처음엔 비워둔다.

---

## 5. 표현 대응표

| 한국어 입력 | 영어 출력 | 쓰지 말 것 |
|---|---|---|
| 근경 / 중경 / 클로즈업 | medium shot / mid-ground / close-up | — |
| 아이레벨 / 로우앵글 | eye-level / low camera position | — |
| 하드컷 전환 | hard cut to | transition effect |
| 아웃포커스로 걸림 | racked out of focus in the <위치> foreground | blurry background |
| 반 걸음 물러선다 | steps back half a pace | backs away |
| 눈을 부릅뜬다 | eyes go wide and fixed | looks shocked |
| 말을 잃는다 | throat moves, no sound comes out | speechless |
| 문장 끝을 올린다 | rising terminal pitch | — |
| 눌러 참고 자제함 | restrained, holding back | — |
| 맑고 차가운 소년음 | clear, cool boy's voice | — |

**금지 어휘** — 정보량이 0이고 모델을 슬롭 쪽으로 민다:
`cinematic`, `dramatically`, `breathtaking`, `stunning`, `epic`, `masterpiece`,
`highly detailed`(STYLE 블록 밖에서), `4k`(이미 STYLE에 있음), `award-winning`

---

## 6. VOICE PROFILE — 고정층 블록 (필수)

음색과 성격은 **인물 상수**라 컷에 적지 않는다. 컷의 `VOICE:` 줄에는 state와 delivery 두 칸만 쓴다.
같은 상수가 컷마다 반복되면 반드시 어긋난다. 대신 고정층에 이 블록을 한 번 둔다.

위치: CONTINUITY 뒤, 첫 SHOT GROUP 앞.

```
VOICE PROFILE
@<인물A> — <음색 한 구절>. <성격 한 구절>.
@<인물B> — <음색 한 구절>. <성격 한 구절>.
Timbre and temperament are declared once here. Each shot carries state and delivery only.
```

마지막 줄을 빼면 모델이 컷마다 음색을 새로 정한다.

## 7. SPACE LOCK · DEPTH TIERS · FRAMING

배경 구역 분할, 컷별 FRAMING, 심도 등급, 발광 배경 처리는 `scene-space.md` 에 전문이 있다.
블록 위치만 여기 적는다.

- SPACE LOCK — CONTINUITY 뒤, VOICE PROFILE 앞
- DEPTH TIERS — LIGHTING 안에 접거나 그 바로 뒤에 독립 블록으로
- FRAMING — 각 컷의 action 아래, `Show only what is described…` 위
