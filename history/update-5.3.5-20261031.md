# BocaBoa 업데이트 내역 - v5.3.5

- **업데이트 일자**: 2026년 10월 31일
- **대상 버전**: v5.3.5
- **저장소**: [creatorjosephkr/bocaboa](https://github.com/creatorjosephkr/bocaboa)

---

## 🚀 주요 변경 사항

### 1. AI 로스팅 가이드 모드 로스팅 종료 후 메인 앱 포커스 이동 차단
- **3초 비활성 타이머(`aiInactivityTimer`) 잔존 버그 해결**:
  - 로스팅 진행 중(목표 예열 도달 ~ 원두 배출)에는 단축키 수용을 위해 AI 창에서 3초간 입력이 없으면 메인 앱으로 포커스를 복원하는 타이머가 작동합니다.
  - 로스팅 종료 시(`triggerFinishing()`) 예열 도달 플래그(`hasReachedTargetPreheat`)가 리셋되지 않아 로스팅 흐름 비활성화가 정상 전달되지 못했고, 로스팅이 끝난 뒤에도 AI 창을 클릭할 때마다 3초 뒤 메인 앱으로 포커스를 빼앗아 가던 문제를 수정했습니다.
- **로스팅 종료 프롬프트 주입 시 강제 포커스 전환 제거**:
  - AI 가이드 창으로 `[시스템 이벤트 - 로스팅 종료]` 메시지를 전달한 직후, `main.js`의 프롬프트 전송 로직에서 무조건 200ms 뒤 메인 앱으로 강제 포커스(`focusMainWindow(true)`)를 이동시키던 동작을 제거했습니다.
- **메인 윈도우 포커스 복원 가드(`isAiRoastFlowActive`) 강화**:
  - `main.js`의 `focusMainWindow()`, `resetAiInactivityTimer()`, `ai-webview-focus-main`, `focus-main-window`, `relay-ai-shortcut` 등에 `if (!isAiRoastFlowActive) return;` 가드를 추가하여, 로스팅이 끝난 상태에서는 어떠한 호출로도 메인 윈도우가 포커스를 빼앗아가지 못하도록 원천 차단했습니다.
  - 로스팅 진행 상태(`ai-roasting-status`)를 래퍼 창뿐만 아니라 내부 ChatGPT `BrowserView`에도 실시간 브로드캐스트(`sendAiRoastingStatus`)하도록 개선했습니다.
- **내부 웹뷰(ChatGPT/Claude) 및 상단 툴바 포커스 릴레이 차단**:
  - 로스팅이 끝난 상태(`isRoastingActive === false`)에서는 ChatGPT 내부에서 메시지 전송 버튼을 누르거나 키보드를 입력하더라도 메인 앱으로 포커스 전환 요청을 보내지 않도록 차단했습니다.
  - 로스팅 종료 후 사용자가 AI 창에서 ChatGPT와 로스팅 결과 분석 및 후속 질문을 자유롭게 주고받아도 포커스가 메인 창으로 튀지 않고 온전히 유지됩니다.

---

### 📦 변경된 파일
- `app.js`: `triggerFinishing()`에서 `hasReachedTargetPreheat = false` 및 `isRoastDischarged = false` 리셋, 현재 버전 `v5.3.5` 동기화
- `main.js`: `isAiRoastFlowActive` 플래그 관리, `sendAiRoastingStatus` 브로드캐스트 추가, `focusMainWindow()` 및 비활성 타이머 포커스 차단 가드 적용, 로스팅 종료 시 포커스 이동 로직 제거
- `preload-ai-webview-inner.js`: `ai-roasting-status` 상태 동기화, 로스팅 종료 시 단축키 릴레이 및 전송 포커스 전환 차단
- `ai-webview.html`: 로스팅 상태 동기화 및 로스팅 종료 시 키다운 단축키 릴레이 차단
- `index.html`: 업데이트 내역 및 환영 모달 버전 배지 `v5.3.5` 반영
- `ai-start-page.html`: 푸터 버전 `v5.3.5` 반영
- `package.json`, `package-lock.json`, `README.md`: 앱 버전 `v5.3.5` 업데이트
- `history/update-5.3.5-20261031.md`: v5.3.5 릴리즈 노트 신규 생성
