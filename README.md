# my-stock-team

국내 **상업용 부동산 관점**의 종목 리서치를 팀으로 수행하는 Claude Code 플러그인입니다.
애널리스트 서브에이전트 5종, KB 디자인 PPTX 리포트 스킬, 리서치 오케스트레이션 커맨드를 묶었습니다.

## 구성

```
my-stock-team/
├── .claude-plugin/
│   ├── plugin.json          # 플러그인 매니페스트 (name: my-stock-team, v1.0.0)
│   └── marketplace.json     # 설치 카탈로그
├── agents/                  # 애널리스트 서브에이전트 5종
│   ├── fundamental-analyst.md       # 재무·실적·공시 (DART)
│   ├── market-technical-analyst.md  # 주가·추세·변동률 (FinanceDataReader)
│   ├── news-sentiment-analyst.md    # 뉴스·시장 심리 (웹)
│   ├── risk-manager.md              # 리스크 종합·유동성 (pykrx)
│   └── report-quality-auditor.md    # 전달 전 품질 검증
├── skills/
│   └── report-pptx/         # 리서치 .md → KB 디자인 PPTX
│       ├── SKILL.md
│       └── render.py
└── commands/
    └── research.md          # /research <종목명> — 전체 흐름 오케스트레이션
```

## 설치

```
# 1) 마켓플레이스 등록 (이 폴더 경로 또는 git 저장소)
/plugin marketplace add ./my-stock-team

# 2) 설치
/plugin install my-stock-team@my-stock-team
```

## 사용

```
/research 율촌화학            # 분야별 분석(병렬) → 리스크 종합 → 저장 → 검증 → PPTX
/report-pptx 율촌화학         # 이미 작성된 reports/율촌화학.md → reports/율촌화학.pptx
```

개별 애널리스트는 "삼성전자 최근 공시 분석해줘"처럼 자연어로 요청하면 자동 위임됩니다.

## 사전 준비 (사용자 환경)

- Python 패키지: `python-pptx`, `OpenDartReader`/`requests`, `FinanceDataReader`, `pykrx`
- **DART OpenAPI 키**: 본 플러그인은 비밀값을 포함하지 않습니다. 사용자가 자신의 프로젝트
  `.env`에 `DART_KEY=<발급키>`를 직접 설정해 사용합니다. (키 발급: opendart.fss.or.kr)

## 가드레일 (모든 리포트 항상 적용)

- 모든 수치에 `(출처: 데이터명, 연도/날짜)` 병기 — 출처 없는 수치는 쓰지 않음
- 데이터 미확보 시 "확인 불가", 출처 불명 뉴스·루머는 "미확인"
- 매수/매도/보유·목표가·비중 단정 금지 — 판단 근거(의사결정 지원)까지만
- 첫머리 "무료 공개 데이터 기반 학습용" 한 줄, 끝에 데이터 출처·기준일 목록

본 플러그인의 산출물은 학습용 분석 자료이며, 투자 권유나 자문이 아닙니다.
