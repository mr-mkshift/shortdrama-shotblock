# shortdrama-shotblock

숏드라마·시리즈물을 **공장식으로** 찍어내기 위한 분경조(分镜组) 컴파일러 스킬.
한국어로 적은 컷 정보를 Dreamina·Seedance·Doubao용 **중국어** 프롬프트나
Veo·Sora·Runway용 **영어** 프롬프트로 컴파일한다.

A Claude Code skill that compiles Korean shot notes into Chinese or English
video-generation prompts, built for producing episodic short drama at volume.

---

## 왜 있는가

숏드라마는 한 편의 예술품이 아니라 80편짜리 생산 라인이다. 그래서 목표가 다르다.

- 100%의 일관성을 시간 들여 잡는 게 아니라, **90%를 한 번의 생성으로** 잡는다
- 리롤 비용과 편집 시간을 줄이는 쪽으로 프롬프트 구조를 짠다
- 컷과 컷, 화와 화 사이의 연결을 사람이 확인하지 않아도 굴러가게 만든다

여기서 원칙 하나가 나온다 — **바꿀 곳을 미리 정해둔다.**
프롬프트를 고정층과 변동층으로 쪼개면, 80편을 찍으면서 손대는 곳이 컷 표 몇 칸으로 줄어든다.

## 무엇을 하는가

| | |
|---|---|
| **고정층 / 변동층 분리** | 스타일·조명·소재 바인딩·음색은 1회. 컷마다 채우는 건 상태·어조·대사뿐 |
| **조명을 비율로 잠금** | "번쩍이는" 같은 형용사 대신 재질 상대비 `금속 ×1.5 / 피부 ×0.5 / 천 ×0.3` |
| **배경을 구역으로 분할** | 참조도는 재질만 잠그고 구도는 컷별 취경 한 줄이 정한다. 근접에서 배경이 안 튄다 |
| **발화 타임라인 연출** | 카메라 무빙 대신 대사의 특정 단어 시점에 동작을 건다 |
| **더빙 상수/변수 분리** | 음색·성격은 인물에 붙박이, 상태·어조만 컷 변수 |
| **텍스트 인계** | 종료 상태를 텍스트로 고정해 다음 화가 그대로 받는다 |
| **프로젝트 카드** | 이번 작품의 고정층과 예외를 카드로 떠서 다음 세션·다른 에이전트에 넘긴다 |

## 설치

Claude Code · Codex · ChatGPT Custom GPT 세 환경에서 쓸 수 있다. 자세한 절차는 [INSTALL.md](INSTALL.md).

```bash
# Claude Code
git clone https://github.com/mr-mkshift/shortdrama-shotblock.git ~/.claude/skills/shortdrama-shotblock

# Codex — 클론 후 ~/.codex/AGENTS.md 에서 이 리포의 AGENTS.md 를 참조
git clone https://github.com/mr-mkshift/shortdrama-shotblock.git ~/skills/shortdrama-shotblock
```

## 구성

```
SKILL.md                          컴파일 절차·검수·출력 계약
references/grammar-zh.md          중국어 블록 명칭·순서·템플릿
references/grammar-en.md          영어판
references/character-bible.md     캐릭터 상수표, 캐릭터시트 훼손 사양
references/lighting-materials.md  재질 상대비 설계, 장면 유형별 배합표
references/scene-space.md         배경 구역 분할, 컷별 취경, 발광 배경, 화면 내 문자
references/project-card.md        프로젝트 상수를 다음 세션·다른 에이전트에 넘기는 그릇
AGENTS.md                         Codex·Custom GPT용 구속 지시문 (출력 계약·게이트)
INSTALL.md                        환경별 설치 절차
```

## Claude가 아닌 에이전트에서 쓰기

`AGENTS.md` 가 그 역할을 한다. Codex는 리포 루트에서 이 파일을 자동으로 읽고,
Custom GPT는 전문을 Instructions 란에 붙여넣는다. 문법 정본은 양쪽 다 `references/` 다.

Claude 외의 모델은 출력 계약을 잘 이탈해서, 리터럴 금지 목록(§2)과
출력 직전 게이트(§11), 참조 파일 조회 의무(§5)를 따로 넣어뒀다.

## 출력 형태

```
[분경조]     타깃 언어 전문. 그대로 복사해 생성기에 넣는다
[대조]       같은 내용의 한국어 검수표
[인계 카드]  다음 화에 붙여넣을 종료 상태 + 첫 프레임 + 첫 대사
```

## 대상이 아닌 것

단일 이미지 프롬프트, 룩북 모델 얼굴 생성, 캐릭터시트 이미지 제작 자체.
이 스킬은 **영상 컷 블록**만 다룬다.

## 라이선스

MIT
