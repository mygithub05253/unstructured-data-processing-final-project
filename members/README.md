# 팀 작업 폴더 (Team Workspaces)

본 ESG DART 프로젝트는 **4명이 각자 전체 파이프라인을 개별 진행한 뒤 통합하는 방식**으로 운영됩니다.

## 폴더 구조

| 폴더 | 역할 | 비고 |
|---|---|---|
| `이동원/` | 개인 작업물 | 본인 파이프라인 + GitHub 관리 |
| `김지우/` | 개인 작업물 | 본인 파이프라인 |
| `김혜성/` | 개인 작업물 | 본인 파이프라인 |
| `신지영/` | 개인 작업물 | 본인 파이프라인 |
| `_integrated/` | 4명 통합 결과 | 최종 분석·발표 자료 (5/19 이후) |

## 워크플로우

1. 각 팀원은 본인 폴더에서 **전체 파이프라인**을 작업 (수집 → 전처리 → 마이닝 → 분석)
2. 작업물을 **공유 드라이브**에 업로드
3. 관리자(이동원)가 본 GitHub 레포에 일괄 push
4. 5/19 데이터 quality 게이트 통과 후 → 5/20 이후 `_integrated/`에서 베스트 부분 통합

## 각 개인 폴더 표준 구조

```
{이름}/
├── notebooks/         # Jupyter 노트북 (01_collection_v1.ipynb, 01_collection_v2.ipynb …)
│   └── archive/       # 이전 버전 스냅샷 (규칙 1 rollback)
├── src/               # Python 모듈 (재사용 함수)
├── reports/           # 분석 노트, 의사결정 기록, 회의록 등
└── README.md          # 본인의 작업 진행 상황·로그
```

## 노트북 작업 6개 규칙 (★ 필수)

모든 .ipynb 파일은 아래 6개 규칙을 지킨다. 위반 시 PR reject.

| # | 규칙 | 적용 |
|---|---|---|
| 1 | **버전 보존** | 새 버전 만들 때 기존 파일 덮어쓰기 X. `01_collection_v2.ipynb` 새로 만들거나 `archive/`로 백업 |
| 2 | **GitHub에 출력 보존** | `nbstripout` 사용 금지. 셀 출력 그대로 commit |
| 3 | **진행률 시각화** | `tqdm.notebook` 또는 ipywidgets progress bar 필수 |
| 4 | **교수님 가이드 준수 + 모든 경우의 수 테스트** | docs/professor_md/01~04 위반금지 8항목 헤더 셀 + alt 셀로 변형 실험 |
| 5 | **결과 시각화** | matplotlib·seaborn·plotly·wordcloud 등 각 결과마다 시각화 셀 |
| 6 | **상세 마크다운·주석** | 각 셀 위에 What / Why / 근거 (교수님 가이드 인용) 3블록 마크다운 |

## 버전 관리 표준 (규칙 1)

**산업 표준 — 하이브리드 방식**

```
notebooks/
├── 01_collection_v1.ipynb     ← 1차 작성 (출력 포함)
├── 01_collection_v2.ipynb     ← 발전된 버전 (출력 포함)
├── 01_collection_v3.ipynb     ← 최신 active
└── archive/                   ← 더 이상 비교 안 하는 옛 버전 보관
    └── 01_collection_v0_초기파일럿_2026-05-15.ipynb
```

- **소폭 발전**: `_v1` → `_v2` → `_v3` 같은 폴더에 누적
- **대폭 발전 후 정리**: 4-5개 누적되면 옛 버전을 `archive/`로 이동
- **결과 악화 시**: 직전 `_vN-1.ipynb` 그대로 사용 (git revert 없이 즉시 rollback)
- **archive 파일명**: `{원본명}_{YYYY-MM-DD}_{descriptor}.ipynb` (예: `01_collection_2026-05-15_초기파일럿.ipynb`)

## 노트북 표준 헤더

매 노트북 최상단 마크다운 셀에 표준 헤더를 둠. 템플릿: [`_TEMPLATE_노트북_표준헤더.md`](_TEMPLATE_노트북_표준헤더.md)

## 데이터 파일 정책

- 각 폴더의 `notebooks/` · `src/` · `reports/`는 **코드/문서만** 커밋
- 원본 데이터(zip · json · parquet · csv)는 모두 `.gitignore` (각 팀원 로컬·공유 드라이브 보관)
- `analytical_dataset.parquet` 등 FREEZE 산출물은 `_integrated/`에 hash만 기록
- 노트북 셀 출력은 보존 (규칙 2) — 단 **API key는 절대 출력에 노출 금지** (`<MASKED>` placeholder 사용)

## 왜 이렇게 나누었나

- **버전 관리** — 각 팀원의 작업 history 보존
- **파일 보존** — 개인 중간 결과물 손실 방지
- **통합 시점 명확화** — `_integrated/`로 옮기는 순간 = 합의된 산출물
- **실패 격리** — 한 사람의 코드 깨짐이 다른 사람 폴더에 영향 없음
- **상호 학습** — 4명이 같은 문제를 다르게 풀어보고 베스트 비교

## 참고 가이드

- 데이터 수집 가이드: [`docs/supplements/versions/v0.2/COLLECTION_데이터수집가이드.html`](../docs/supplements/versions/v0.2/COLLECTION_데이터수집가이드.html)
- 5/15~5/19 5일 로드맵: [`docs/supplements/versions/v0.2/ROADMAP_5일_데드라인.html`](../docs/supplements/versions/v0.2/ROADMAP_5일_데드라인.html)
- 식별자 규칙·PR 정책: [`CONTRIBUTING.md`](../CONTRIBUTING.md)
