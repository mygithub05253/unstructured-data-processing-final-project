# 노트북 표준 헤더 템플릿

매 `.ipynb` 파일 **최상단 마크다운 셀**에 아래 블록을 복사·붙여넣기. 비어 있는 부분(`{ ... }`)만 본인 작업에 맞춰 채움. 본 템플릿 자체는 markdown 미리보기용이며, 실제 노트북에는 **코드 펜스 안 내용만** 들어감.

---

## 셀 1 (markdown) — 노트북 메타 정보

```markdown
# {N}_{단계명}.ipynb — v{버전번호}

> **단계**: {수집 / 전처리 / 마이닝 / 분석}  
> **작성자**: {이동원 / 김지우 / 김혜성 / 신지영}  
> **작성 일자**: 2026-05-{DD}  
> **버전**: v{N}  
> **이전 버전과의 차이**: {bullet 한 줄, 예: "섹션 II 정규식 추가 / KoNLPy → Mecab swap"}

## 이 노트북에서 하는 일 (What)

- {짧은 bullet 3-5개로 노트북의 입력·처리·출력 요약}
- 예시:
  - 입력: `data/company_master_filled.csv` (127 firm × 3 year = 381 firm-year)
  - 처리: OpenDART API → document.xml → 섹션 II/IV/VI 추출 → ESG passage 후보
  - 출력: `data/raw/{stock}_{year}_disclosure.zip` + `data/interim/{stock}_{year}_sections.json` + `logs/collection_log.csv`

## 왜 이렇게 했나 (Why)

- {각 결정의 근거를 2-3문장으로}
- 예시: stock_code로만 join하는 이유는 회사명 변경 시 silent mismatch 위험 (data/README.md 명시). esg_year = fiscal_year + 1로 둔 이유는 KCGS 발표 시점이 fiscal year 다음 해 상반기 (출처: 04_submission_and_evaluation.md §3-2)

## 출처·근거 (Reference)

- [ ] `docs/professor_md/01_assignment_overview.md` — {참조한 절}
- [ ] `docs/professor_md/02_dart_data_collection.md` — {참조한 절}
- [ ] `docs/professor_md/03_minimal_analysis_example.md` — {참조한 절}
- [ ] `docs/professor_md/04_submission_and_evaluation.md` — {참조한 절}
- [ ] `docs/supplements/versions/v0.2/COLLECTION_데이터수집가이드.html` — {참조한 탭}
- [ ] `docs/supplements/versions/v0.2/ROADMAP_5일_데드라인.html` — {Day {N} 카드}
- [ ] {학술 논문 / Stack Overflow 등 외부 출처가 있으면 명시}
```

---

## 셀 2 (markdown) — 위반 금지 8항목 체크

```markdown
## 위반 금지 8항목 체크 (교수님 가이드)

본 노트북이 아래 8항목을 모두 통과함을 self-check.

- [x] (1) 모든 크롤링 경로에 출처·lineage 기록
- [x] (2) document.xml이 표준 raw 텍스트 경로 (우회 금지)
- [x] (3) MCP는 발견·연습용, corpus 투입 전 Python 재현 검증
- [x] (4) API key 코드 노출 없음 (.env만 사용, res.url 출력 금지)
- [x] (5) rate limit·backoff·캐시 적용
- [x] (6) 실패 행은 collection_log.csv에 기록 (가짜 0 금지)
- [x] (7) stock_code 기준 join (회사명 join 금지)
- [x] (8) esg_year = fiscal_year + 1 매핑

## 노트북 운영 6개 규칙 체크

- [x] (1) 버전 보존 — 본 파일은 v{N}, 직전 버전은 archive/ 또는 _v{N-1}.ipynb로 보존
- [x] (2) 셀 출력 보존 — nbstripout 미사용. API key는 <MASKED> placeholder만 노출
- [x] (3) 진행률 시각화 — tqdm.notebook 또는 ipywidgets로 오래 걸리는 작업 표시
- [x] (4) 모든 경우의 수 테스트 — alt 셀로 {예: 형태소 분석기 3종 / vocab 크기 sweep} 비교
- [x] (5) 결과 시각화 — 각 단계 결과를 matplotlib/seaborn/plotly/wordcloud로 시각화
- [x] (6) 마크다운·주석 상세 — What/Why/근거 3블록 + 비전공자도 이해 가능한 주석
```

---

## 셀 3 (code) — 환경 셋업 + 시각화 import 표준

```python
# ─────────────────────────────────────────────
# 환경 셋업 — 모든 노트북 공통
# ─────────────────────────────────────────────
import os
import sys
from pathlib import Path
from datetime import datetime

# 환경 변수 (API key 등)
from dotenv import load_dotenv
load_dotenv()  # .env 파일에서 OPENDART_API_KEY 등 로드

# 데이터 처리
import pandas as pd
import numpy as np

# 진행률 시각화 (규칙 3)
from tqdm.notebook import tqdm  # ← ipywidgets 필요

# 결과 시각화 (규칙 5)
import matplotlib.pyplot as plt
import seaborn as sns
import koreanize_matplotlib  # 한국어 폰트 자동 설정

sns.set_theme(style='whitegrid', context='notebook')

# 재현성
import random
SEED = 42
random.seed(SEED)
np.random.seed(SEED)

# 노트북 메타
NOTEBOOK_NAME = "{N}_{단계명}_v{버전번호}"
NOTEBOOK_AUTHOR = "{이름}"
NOTEBOOK_DATE = datetime.now().strftime("%Y-%m-%d %H:%M")

print(f"[{NOTEBOOK_NAME}] {NOTEBOOK_AUTHOR} · {NOTEBOOK_DATE}")
print(f"Python {sys.version.split()[0]} · pandas {pd.__version__} · numpy {np.__version__}")
```

---

## 셀 4 (markdown) — 데이터 흐름도

```markdown
## 데이터 흐름 (이 노트북의 입출력)

```
{입력 파일/소스}  ──►  [본 노트북]  ──►  {출력 파일}
                                  │
                                  └──►  {로그/메타}
```

예시 (01_collection):
```
company_master_filled.csv  ──►  [01_collection]  ──►  data/raw/*.zip + data/interim/*.json
OpenDART API (corpCode·list·document)              └──►  logs/collection_log.csv
```
```

---

## 마지막 셀 (markdown) — 노트북 종료 요약

```markdown
## 이번 실행 요약

- **실행 시각**: {YYYY-MM-DD HH:MM}
- **처리 건수**: {N firm-years}
- **성공 / 실패**: {N_success} / {N_fail}
- **다음 단계**: {02_preprocessing.ipynb 등}

### 다음 버전(v{N+1})에서 개선할 것

- {bullet 2-4개}
- 예시:
  - 정정공시 last_reprt_at=Y 처리 누락 → 다음 버전에서 추가
  - 섹션 II 정규식이 일부 firm-year에서 깨짐 → 패턴 alt 추가 검토
```

---

## 사용 예시

새 노트북 `members/이동원/notebooks/01_collection_v2.ipynb`를 만들 때:

1. 본 템플릿의 셀 1-4 + 마지막 셀을 복사해서 새 노트북에 붙여넣기
2. `{...}` 부분을 본인 작업에 맞게 채움
3. 이전 버전 `01_collection_v1.ipynb`는 그대로 보존 (또는 `archive/`로 이동 시 `2026-05-{DD}_{descriptor}` 형태로 rename)
4. 출력 셀 보존 상태로 commit
5. 셀 출력에 API key 등 비밀값이 노출되지 않았는지 self-check
