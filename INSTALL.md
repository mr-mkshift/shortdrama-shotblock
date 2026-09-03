# 설치

세 가지 환경에 설치할 수 있다. 문법 정본은 어느 쪽이든 `references/` 의 md 6장이다.

| 환경 | 읽는 파일 | 설치 방식 |
|---|---|---|
| Claude Code (데스크톱·CLI) | `SKILL.md` + `references/` | 스킬 폴더에 클론 |
| Codex (ChatGPT 데스크톱) | `AGENTS.md` + `references/` | 리포 클론 + 전역 참조 |
| ChatGPT Custom GPT (웹) | `AGENTS.md` + `references/` | 지시문 붙여넣기 + Knowledge 업로드 |

---

## A. Claude Code

```bash
git clone https://github.com/mr-mkshift/shortdrama-shotblock.git ~/.claude/skills/shortdrama-shotblock
```

재시작하면 스킬 목록에 잡힌다. 숏드라마 컷·분경조·캐릭터 바이블·다음 화 인계 같은 말이 나오면
자동으로 발동하고, `/shortdrama-shotblock` 으로 직접 부를 수도 있다.

`SKILL.md` 의 프론트매터가 발동 조건을 담고 있으므로 별도 설정이 없다.
`AGENTS.md` 는 Claude Code에서 쓰이지 않는다 — 다른 에이전트용 구속 지시문이다.

---

## B. Codex (ChatGPT 데스크톱 앱)

Codex는 작업 디렉터리의 `AGENTS.md` 를 자동으로 읽는다.
어느 프로젝트에서든 쓰려면 전역 설정에서 이 리포를 가리킨다.

### 1. 클론

```bash
git clone https://github.com/mr-mkshift/shortdrama-shotblock.git ~/skills/shortdrama-shotblock
```

### 2. 전역 AGENTS.md에서 참조

`~/.codex/AGENTS.md` 에 아래를 추가한다. 파일이 없으면 새로 만든다.

```markdown
## 숏드라마 분경조 컴파일

숏드라마·시리즈물의 컷 블록, 분경조, 스토리보드 프롬프트, 캐릭터 바이블,
다음 화 인계가 언급되면 아래를 **답을 쓰기 전에** 수행한다.

1. `~/skills/shortdrama-shotblock/AGENTS.md` 를 읽는다. 참고가 아니라 실행 지시다.
2. 그 문서의 "시작 전" 절차를 그대로 따른다 — 타깃 언어를 정하고
   `~/skills/shortdrama-shotblock/references/` 의 해당 문법 파일을 **실제로 연다.**
3. 문법 파일을 열지 않은 채 분경조를 출력하지 않는다.

출력 형식은 그 문서의 출력 계약을 따른다. 자기 형식으로 재구성하지 않는다.
```

경로는 실제 클론 위치에 맞춘다.

"읽고 따른다" 한 줄만 적으면 Codex가 파일을 안 열고 기억으로 답한다.
**읽는 것을 개별 단계로 쪼개고, 안 읽으면 실패라고 명시**해야 실제로 연다.

### 3. 프로젝트 단위로만 쓸 때

해당 프로젝트에서만 쓰려면 리포를 프로젝트 안에 두고,
프로젝트 루트의 `AGENTS.md` 에서 같은 방식으로 참조한다.

---

## C. ChatGPT Custom GPT (웹)

생성은 웹/데스크톱 에디터에서만 된다. 한 번 만들면 모바일 앱 사이드바 GPTs에도 뜬다.

1. chatgpt.com → 좌측 **GPTs** → **+ Create** → 상단 탭 **Configure**
   (Create 탭의 대화형 생성은 쓰지 않는다)
2. **Name** — `분경조 컴파일러`
3. **Description** — `한국어 컷 정보를 중국어/영어 분경조 프롬프트로 컴파일한다.`
4. **Instructions** — `AGENTS.md` 전문을 붙여넣기 (약 5,600자 / 한도 8,000자)
5. **Knowledge** — `references/` 의 md 6장 업로드

   ```
   grammar-zh.md  grammar-en.md  character-bible.md
   lighting-materials.md  scene-space.md  project-card.md
   ```

6. **Capabilities** — **전부 끈다**
   - Web Search ✗ · Canvas ✗ · DALL·E ✗ · Code Interpreter ✗
   - 켜져 있으면 컴파일 대신 검색하거나 캔버스를 열어 출력 계약이 깨진다
7. **Conversation starters**

   ```
   분경조 컴파일 시작
   캐릭터 바이블부터 만들자
   이전 인계 카드 이어서
   대조 빼고 분경조만
   ```

8. 우상단 **Create** → 공개 범위 `Only me`

### 프로젝트로 쓸 때

모바일 앱에서도 만들 수 있다. 구속력은 Custom GPT보다 약간 무르다 —
프로젝트 지시문은 GPT Instructions보다 모델이 느슨하게 읽는다.

1. 앱 → **프로젝트** → 새 프로젝트
2. **지시문** — `AGENTS.md` 전문 붙여넣기
3. **파일** — `references/` 의 md 6장 첨부
4. 프로젝트 안에서만 대화한다 (일반 대화창에서는 지시문이 안 걸린다)

느슨해졌다 싶으면 첫 메시지 맨 앞에 붙인다.

```
출력 계약과 §11 게이트를 그대로 적용한다. 블록 밖에 아무것도 쓰지 않는다.
```

---

## 동작 확인

설치 후 이걸 넣어본다.

```
안녕? 뭐 할 수 있어?
```

**통과** — `[부족]` 블록이 나오거나, 타깃·총길이·캐릭터 재료를 묻는 항목만 나온다
**실패** — "안녕하세요! 저는 숏드라마 분경조를..." 하고 자기소개가 나온다

Custom GPT에서 실패하면 Capabilities가 켜져 있는지부터 본다. 특히 Canvas.
Codex에서 실패하면 `AGENTS.md` 가 실제로 로드됐는지 확인한다.

---

## Claude와 다른 에이전트의 차이

| 항목 | Claude Code | Codex · Custom GPT |
|---|---|---|
| 발동 | `SKILL.md` description 매칭으로 자동 | 항상 켜짐 |
| 참조 파일 | 필요할 때 읽음 | 맨 위 "시작 전" 블록에서 **여는 것을 개별 단계로** 못 박아 기억 답변을 막음 |
| 출력 이탈 | 드묾 | 잦음 → §2 리터럴 금지, §11 게이트, §13 흔한 이탈 표 |
| 부분 컴파일 | 알아서 물음 | §4 `[부족]` 단일 출력으로 강제 |
| 컷 길이 검산 | 대체로 맞음 | §11-1에 "실제로 더해서 검산" 명시 |
