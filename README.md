# Unreal Action Survival Prototype

> Unreal Engine 5 Blueprint로 전투, 적 AI, 상호작용과 보상 시스템을 구현한 개인 액션·탐험 프로토타입입니다.

## 프로젝트 개요

| 항목 | 내용 |
|---|---|
| 개발 기간 | 2025.05.24 ~ 2025.06.20 |
| 인원 | 1명 |
| 엔진 | Unreal Engine 5.5 |
| 구현 방식 | Blueprint-only, Enhanced Input, Behavior Tree / Blackboard |
| 대상 플랫폼 | Windows, DirectX 12, Shader Model 6 |
| 현재 상태 | Blueprint 구조 정적 확인, UE 5.5 실행·패키징 확인 필요 |

외부 모델과 환경 에셋을 직접 제작하는 대신, 플레이어 전투와 AI, 상자·아이템·광물·코인, HUD와 사망 흐름을 Blueprint로 구성하고 하나의 레벨에 통합했습니다.

## 핵심 구현

### 플레이어와 전투

- Enhanced Input 기반 이동·점프·달리기
- 검·곡괭이 장착 전환과 Aim Mode
- 공격 Montage와 상·하체 Animation 분리
- Line/Sphere Trace 기반 공격 판정과 `ApplyDamage`
- HP, 코인, 공격력, 점프 수치 관리

### 적 AI

- Behavior Tree, Blackboard, AI Controller 구성
- 랜덤 순찰에서 플레이어 탐색·추적으로 이어지는 상태 전환
- 거리 기반 공격 가능 여부와 이동 속도 갱신
- 고블린·골렘별 공격, 피격·사망, 코인 드롭
- 데미지 숫자 팝업으로 전투 결과 표시

### 상호작용과 보상

- `BPI_Interact`, `BPI_ItemEffect` Interface로 상호작용 분리
- 상자 개봉과 랜덤 아이템 생성
- 덤벨의 공격력 증가, 스프링의 점프 수치 증가
- 광물 피격·파괴와 코인 생성·획득
- 이름·설명·아이콘·타입을 담는 `ItemData` 구조체

### UI와 레벨

- HP·코인·경과 시간 HUD와 능력치 상태창
- START / QUIT 타이틀 UI
- 사망 UI와 타이틀 복귀·게임 종료 흐름
- 동굴 입구, 고블린 마을, 골렘 사원과 산악 지형으로 구성한 `MainMap`

## 구현 위치

| 영역 | 주요 에셋 |
|---|---|
| 플레이어 | [`Content/Blueprints/Characters`](Content/Blueprints/Characters) |
| AI | [`Content/Blueprints/enemy`](Content/Blueprints/enemy) |
| 상호작용 | [`Content/Blueprints/Interfaces`](Content/Blueprints/Interfaces) |
| 아이템·보상 | [`Content/Blueprints/Items`](Content/Blueprints/Items) |
| 상자·광물 | [`Chests`](Content/Blueprints/Chests), [`BreakableObject`](Content/Blueprints/BreakableObject) |
| UI | [`Content/Blueprints/UI`](Content/Blueprints/UI) |
| 맵 | [`Content/Blueprints/map`](Content/Blueprints/map) |

## 실행 방법

### 요구 사항

- Unreal Engine **5.5**
- Windows 10/11
- DirectX 12와 Shader Model 6을 지원하는 GPU
- 저장소 약 2.43GiB를 받을 수 있는 디스크 공간

### 에디터 실행

1. 저장소를 clone합니다.
2. 루트의 `.uproject` 파일을 Unreal Engine 5.5로 엽니다.
3. Content Browser에서 `Content/Blueprints/map/titleMap.umap`을 엽니다.
4. Blueprint를 Compile한 뒤 Play In Editor로 실행합니다.

현재 기본 시작 맵은 Third Person Template Map으로 설정되어 있어, 설정을 수정하기 전에는 `titleMap`을 직접 열어야 합니다.

## 개발 상태

Blueprint 시스템은 구성되어 있으며, 정확한 UE 5.5 환경에서 Compile All, 전체 플레이 흐름과 Windows 패키징을 최종 확인해야 합니다.

## 외부 에셋

이 저장소에는 Epic Starter Content, Quixel/Megascans, Soul: Cave, Fab 모델과 사운드 팩 등 외부 콘텐츠가 포함되어 있습니다. 해당 에셋은 직접 제작물로 주장하지 않으며, 공개 배포 전 각 상품 페이지의 라이선스와 원본 자산 재배포 조건을 별도로 확인해야 합니다.

- [Soul: Cave — Fab](https://www.fab.com/listings/75f42402-40bb-4a1b-b557-18e2c9604273?lang=en)
- [Epic Content License Agreement](https://www.unrealengine.com/eula/content)

## 프로젝트 구조

```text
.
├─ Config/                         # 맵·렌더러·플랫폼 설정
├─ Content/
│  ├─ Blueprints/
│  │  ├─ Characters/              # 플레이어·장비·Animation
│  │  ├─ enemy/                   # AI·고블린·골렘
│  │  ├─ Interfaces/              # 상호작용·아이템 효과
│  │  ├─ Items/                   # 아이템·코인·데이터
│  │  ├─ Chests/                  # 상자
│  │  ├─ BreakableObject/         # 광물
│  │  ├─ UI/                      # HUD·상태창·타이틀·사망
│  │  └─ map/                     # titleMap·MainMap
│  ├─ Fab/                        # 외부 Fab 콘텐츠
│  ├─ SoulCave/                   # 외부 환경 콘텐츠
│  └─ StarterContent/             # Epic Starter Content
├─ *.uproject
└─ README.md
```

