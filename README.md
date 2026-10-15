# 나만의 로또 — 개인용 모바일 웹앱

## 포함 기능
- 고정 A/B 번호 수정 (초기값 모두 `1, 4, 8, 11, 15, 26`)
- 주간 추천 번호 생성 및 Cloudflare Worker의 월요일 추천 불러오기
- 구매 이력, 누적 지출, 월 예산 관리
- JSON 백업/복원
- 당첨 번호 직접 입력 또는 Worker 결과 API 조회 후 일치 번호/등수 비교
- 공식 동행복권 사이트 링크

## 주의
- 실제 로또 구매/결제 자동화는 구현하지 않았습니다. 공식 사이트에서 허용되는 구매 절차를 직접 이용하세요.
- 비밀번호, 인증 쿠키, 결제 정보는 앱에 저장하지 않습니다.
- 모든 조합의 당첨 확률은 같으며, 과거 통계가 확률을 높여주지 않습니다.
- 기록은 브라우저 `localStorage`에 저장됩니다. 기기 변경/데이터 삭제 전 백업하세요.
- 비공식 개인용 도구이며 동행복권과 제휴하지 않습니다.

## GitHub Pages 배포
1. 저장소 `Settings → Pages`를 엽니다.
2. `Deploy from a branch` 선택 후 `main` 및 `/(root)`를 지정합니다.
3. 저장 후 배포 완료를 기다립니다. 주소는 `https://jeehjung.github.io/lotto/` 형식입니다.

## Cloudflare Worker 선택 설정
1. Cloudflare 대시보드에서 KV namespace를 생성합니다.
2. KV namespace ID를 `wrangler.toml`의 `REPLACE_WITH_YOUR_KV_NAMESPACE_ID`에 넣습니다.
3. 이 폴더에서 `npm install -D wrangler`, `npx wrangler login`, `npx wrangler deploy`를 실행합니다.
4. `https://<worker-name>.<account>.workers.dev/api/health`가 응답하는지 확인합니다.
5. 앱의 `Cloudflare Worker 주소` 칸에 Worker URL을 넣고 `Worker 추천 번호 불러오기`를 누릅니다.
6. Cron은 월요일 00:00 UTC, 한국시간 월요일 오전 9시에 추천을 생성합니다. 최초 실행 전에는 추천 API에 결과가 없을 수 있습니다.

## API
- `GET /api/health`
- `GET /api/results?round=1246`
- `GET /api/recommendation`

Worker의 공개 API는 당첨 결과 읽기와 추천 제공만 합니다. 구매/결제 엔드포인트는 없습니다.
