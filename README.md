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

## 설치

```bash
git clone https://github.com/<user>/shortdrama-shotblock.git ~/.claude/skills/shortdrama-shotblock
```

Claude Code를 재시작하면 스킬 목록에 잡힌다. 숏드라마 컷·분경조·캐릭터 바이블·
다음 화 인계 같은 말이 나오면 자동으로 발동하고, `/shortdrama-shotblock` 으로 직접 부를 수도 있다.

## 구성

```
SKILL.md                          컴파일 절차·검수·출력 계약
references/grammar-zh.md          중국어 블록 명칭·순서·템플릿
references/grammar-en.md          영어판
references/character-bible.md     캐릭터 상수표, 캐릭터시트 훼손 사양
references/lighting-materials.md  재질 상대비 설계, 장면 유형별 배합표
references/scene-space.md         배경 구역 분할, 컷별 취경, 발광 배경 처리
chatgpt/                          ChatGPT Custom GPT 이식본 (지시문 + Knowledge 5장)
```

## ChatGPT에서 쓰기

`chatgpt/SETUP.md` 참고. `INSTRUCTIONS.md` 를 Custom GPT의 Instructions 란에 붙이고
`chatgpt/knowledge/` 의 md 5장을 Knowledge로 올리면 된다.
ChatGPT는 출력 계약을 잘 이탈해서 리터럴 금지 목록과 출력 직전 게이트를 따로 넣어뒀다.

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
