# ChatGPT 설치 — 숏드라마 분경조 컴파일러

지시문 1장 + Knowledge 5장으로 구성된다. Knowledge는 `references/` 원본을 그대로 올린다.

```
chatgpt/INSTRUCTIONS.md  → GPT의 Instructions 란에 전문 붙여넣기 (3,394자 / 한도 8,000자)
references/grammar-zh.md → Knowledge 업로드
references/grammar-en.md
references/character-bible.md
references/lighting-materials.md
references/scene-space.md
```

Knowledge로 올릴 5장은 `references/` 의 원본을 그대로 쓴다. 사본을 따로 두지 않는다 —
두 벌이 되면 한쪽만 고쳐져서 반드시 어긋난다.

---

## A. Custom GPT (권장)

생성은 웹/데스크톱에서만 됩니다. 한 번 만들어두면 **모바일 앱 사이드바 → GPTs** 에 뜨고
Knowledge 파일도 앱에서 그대로 물립니다.

1. chatgpt.com → 좌측 **GPTs** → **+ Create** → 상단 탭 **Configure** (Create 탭 대화형은 쓰지 않는다)
2. **Name** — `분경조 컴파일러`
3. **Description** — `한국어 컷 정보를 중국어/영어 분경조 프롬프트로 컴파일한다.`
4. **Instructions** — `INSTRUCTIONS.md` 전문을 그대로 붙여넣기
5. **Knowledge** — `references/` 의 md 5개 업로드
6. **Capabilities** — **전부 끈다**
   - Web Search ✗ · Canvas ✗ · DALL·E ✗ · Code Interpreter ✗
   - 이게 켜져 있으면 컴파일 대신 검색하거나 캔버스를 열어 출력 계약이 깨집니다
7. **Conversation starters** — 4개 (아래)
8. 우상단 **Create** → 공개 범위는 `Only me`

### Conversation starters

```
분경조 컴파일 시작
캐릭터 바이블부터 만들자
이전 인계 카드 이어서
대조 빼고 분경조만
```

---

## B. 프로젝트 (앱에서 만들고 싶을 때)

모바일 앱에서도 생성·파일 첨부가 됩니다. 구속력은 Custom GPT보다 약간 무릅니다 —
프로젝트 지시문은 GPT Instructions보다 모델이 느슨하게 읽습니다.

1. 앱 → **프로젝트** → 새 프로젝트
2. **지시문** — `INSTRUCTIONS.md` 전문 붙여넣기
3. **파일** — `references/` 의 md 5개 첨부
4. 프로젝트 안에서만 대화한다 (일반 대화창에서 부르면 지시문이 안 걸린다)

느슨해졌다 싶으면 첫 메시지 맨 앞에 이 한 줄을 붙입니다.

```
출력 계약과 §11 게이트를 그대로 적용한다. 블록 밖에 아무것도 쓰지 않는다.
```

---

## 원본 스킬과 다른 점

| 항목 | Claude Code 스킬 | ChatGPT 판 |
|---|---|---|
| 발동 | description 매칭으로 자동 | 항상 켜짐 (GPT/프로젝트 안에서만) |
| 참조 파일 | `references/` 를 필요할 때 읽음 | Knowledge 검색. **§5에서 조회를 의무화**해 기억 답변을 막음 |
| 출력 이탈 | 드묾 | 잦음 → **§2 리터럴 금지 목록**과 **§11 게이트**를 추가 |
| 부분 컴파일 | 알아서 물음 | **§4 `[부족]` 단일 출력**으로 강제 |
| 컷 길이 검산 | 대체로 맞음 | **§11-1에 "실제로 더해서 검산"** 명시 |

원본 문법 파일에서 한 곳을 고쳤습니다 — 컷의 `配音指令` / `VOICE:` 줄에 있던
`<음색> | <성격>` 슬롯을 제거하고, 고정층 `【配音音色表】` / `VOICE PROFILE` 블록으로 옮겼습니다.
SKILL.md 검수 항목("컷에 음색이 있으면 상수가 두 곳에 생겨 어긋난다")과 템플릿이 어긋나 있었고,
ChatGPT는 템플릿 쪽을 따라가 컷마다 음색을 다시 씁니다.

---

## 동작 확인

설치 후 이걸 넣어봅니다.

```
안녕? 뭐 할 수 있어?
```

**통과** — `[부족]` 블록이 나오거나, 타깃/총길이/캐릭터 재료를 묻는 항목만 나온다
**실패** — "안녕하세요! 저는 숏드라마 분경조를..." 하고 자기소개가 나온다

실패하면 Capabilities가 켜져 있는지부터 봅니다. 특히 Canvas.
