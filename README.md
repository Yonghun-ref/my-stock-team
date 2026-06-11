# my-stock-team

종목명만 던지면 **애널리스트 5명이 협업해 리서치를 쓰고, 검증 후 KB 디자인 PPTX 리포트까지 만들어 주는** Claude Code 플러그인입니다. (상업용 부동산 관점의 종목 분석 팀)

## 설치

```
/plugin marketplace add yonghun-ref/my-stock-team
/plugin install my-stock-team@my-stock-team
```

## 사용 예시

Claude Code에서 이렇게 말하면 됩니다:

```
율촌화학 분석해줘
```

그러면 팀이 순서대로 일합니다:

1. **펀더멘털 / 시장·기술 / 뉴스·심리** 애널리스트가 동시에 분석 (재무·주가추세·뉴스)
2. **리스크 매니저**가 결과를 종합해 핵심 리스크와 의견을 정리
3. `reports/율촌화학.md`로 저장
4. **검증 애널리스트**가 출처·수치·가드레일을 최종 점검
5. **report-pptx 스킬**이 `reports/율촌화학.pptx`(KB 디자인, 맑은 고딕)로 내보내기

이미 작성된 리포트만 PPTX로 바꾸려면 `율촌화학 PPTX로 만들어줘`처럼 요청하면 됩니다.

## ⚠️ DART API 키는 각자 발급해 넣으세요

재무·공시 분석(펀더멘털 애널리스트)은 **DART OpenAPI 키**가 필요합니다. **이 플러그인에는 키가 들어있지 않으니, 각자 발급해서 본인 프로젝트에 설정해야 합니다.**

1. https://opendart.fss.or.kr 에서 무료로 API 키를 발급받습니다.
2. 작업 중인 프로젝트 폴더에 `.env` 파일을 만들고 아래 한 줄을 넣습니다:
   ```
   DART_KEY=발급받은_본인_키
   ```
3. `.env`는 깃에 올리지 마세요(키 노출 방지).

> 주가·추세, 뉴스·심리 분석은 키 없이도 동작합니다. DART 키는 재무·공시 부분에만 필요합니다.

## 필요한 Python 패키지

```
pip install python-pptx FinanceDataReader pykrx requests
```

## 구성

- `agents/` — 펀더멘털 · 시장/기술 · 뉴스/센티먼트 · 리스크 · 검증 애널리스트 5종
- `skills/report-pptx/` — 리서치 `.md` → KB 디자인 PPTX 변환
- `commands/research.md` — `/research <종목명>` 전체 흐름 오케스트레이션

---

본 플러그인의 산출물은 **무료 공개 데이터 기반 학습용 분석 자료**이며, 투자 권유나 자문이 아닙니다.
