# ESG DART 기말 프로젝트

비정형데이터분석 (2026 1학기, 가천대) 기말 팀 프로젝트.
DART 사업보고서의 ESG 언어와 KCGS 등급의 연관성 분석.

## 팀
- A · 코디네이션 · GitHub master
- B · 데이터 수집
- C · 전처리 · Feature
- D · 회귀 · 알파 분석

## 폴더 구조
- data/ — 원본·중간·feature (gitignore, 각자 수집)
- notebooks/ — 분석 노트북 (번호-이니셜-내용)
- src/ — 재사용 함수
- reports/ — .md 보고서
- references/ — 강의 자료·문헌 메모

## 환경
1. Python 3.10+
2. .env 파일에 OPENDART_API_KEY 등록 (본인 인증키)
3. pip install -r requirements.txt
4. nbstripout --install (노트북 conflict 방지)

## 팀 헌법 3개
1. 식별자: stock_code 기준 (회사명 X)
2. 타이밍: esg_year = fiscal_year + 1
3. 실패 행: drop + 이유 기록 (가짜 0 X)

## 마일스톤
- M1 수집 완료: [날짜]
- M2 Feature freeze: [날짜]
- M3 회귀·알파 완료: [날짜]
- M4 발표: [날짜]
