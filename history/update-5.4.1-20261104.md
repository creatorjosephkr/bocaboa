# BocaBoa 업데이트 내역 - v5.4.1

- **업데이트 일자**: 2026년 11월 4일
- **대상 버전**: v5.4.1
- **저장소**: [creatorjosephkr/bocaboa](https://github.com/creatorjosephkr/bocaboa)

---

## 🚀 주요 변경 사항

### 1. AI 로스팅 가이드 모드 - Google Gemini(제미나이) 시스템 이벤트 자동 전송 완벽 지원 및 안정화
- **AI 생각 중(Thinking/Streaming) 오탐 해결 및 전송 지연 방지**:
  - 구글 Gemini 웹페이지의 Angular Material 상단 인디케이터나 백그라운드 UI에 상시 존재하는 `mat-progress-bar`, `div[role="progressbar"]` 및 사이드바 메뉴("활동 일시중지" 등)로 인해 답변 생성 중이 아님에도 `isAiThinkingOrGenerating() === true`로 오판정되던 문제를 해결했습니다.
  - 실제 대화/메시지 영역(`model-response`, `.conversation-container` 등) 내부의 visible 요소이거나 명시적 스트리밍(`data-is-streaming="true"`) 상태일 때만 스트리밍 중으로 인식하도록 감지 로직을 정밀화하여, 시스템 이벤트가 150초 동안 무한 대기 상태에 갇히는 현상을 원천 방지했습니다.
- **전송 버튼 필터링 로직 개선 (`tooltip` 오매칭 차단 해제)**:
  - Gemini의 모든 버튼(전송 버튼 포함)에 부여되는 Angular Material 툴팁 디렉티브 클래스(`mat-mdc-tooltip-trigger`, `mat-tooltip-trigger`)로 인해, 불필요한 액션 버튼 배제 필터(`blockedWords`)의 `'tool'` 키워드에 단순 포함되어 전송 버튼 클릭이 스킵되던 치명적 결함을 수정했습니다.
  - 툴팁 관련 클래스를 제외하고 전송 버튼(보내기/전송/Send/Submit)으로 식별된 버튼은 unwanted 필터에서 면제 처리하여 신속하고 안정적인 자동 클릭을 보장합니다.
- **Gemini `<rich-textarea>` 및 Quill ContentEditable 텍스트 주입/개행 최적화**:
  - Gemini 고유의 `<rich-textarea>` 커스텀 엘리먼트 및 Shadow DOM 구조를 완벽하게 탐색하도록 개선했습니다.
  - 줄바꿈(`\n`)이 포함된 긴 시스템 이벤트 메시지 주입 시 Selection과 Range를 명시적으로 잡고 `DataTransfer` 기반 `paste` 이벤트를 최우선 발송하여 에디터 내부 데이터 모델(Delta)에 온전히 반영되도록 처리했습니다. 이를 통해 비어 있는 상태로 인식되어 전송 버튼이 비활성화(disabled) 상태로 유지되던 문제를 해결했습니다.
- **Gemini 한국어/영어 플레이스홀더 및 전송 버튼 셀렉터 보강**:
  - 한국어 Gemini의 *"여기에 프롬프트 입력"*, *"여기에 프롬프트를..."* 및 최신 전송 버튼 셀렉터(`button.send-button`, `button[aria-label*="프롬프트 보내기"]`, `button[aria-label*="메시지 보내기"]`, `button[mattooltip*="프롬프트 보내기"]`, `button:has(mat-icon[fonticon="send"])` 등)를 대폭 추가하여 프롬프트 텍스트 오염을 방지하고 정확한 폼 제출을 지원합니다.
  - 전송 시도 후 텍스트 잔존 여부를 확인할 때도 Gemini 에디터 셀렉터 및 플레이스홀더 제외 검사를 적용하고 보조 네이티브 Enter 이벤트를 발송하여 전송 누락을 완벽히 차단했습니다.

---

### 2. 앱 버전 v5.4.1 판올림 및 시스템 동기화
- 앱 시작 안내 모달(Welcome Modal), 상단 바, 업데이트 내역 모달 및 패키지 설정 전반에 `v5.4.1` 반영
- 업데이트 내역 뷰어에서 현재 버전(`v5.4.1`) 뱃지 자동 인식 및 동기화

---

### 📦 변경된 파일
- `preload-ai-webview-inner.js`: `isAiThinkingOrGenerating()` 오탐 방지, `doActualSubmit()` Gemini 전송 버튼 셀렉터 보강 및 툴팁 필터 예외 처리, `insertPrompt()` Gemini `<rich-textarea>` 지원 및 Selection/Range 기반 `paste` 주입 개선
- `main.js`: `injectScript` Gemini 셀렉터 및 플레이스홀더 보강, `clickScript` Gemini 전송 버튼 셀렉터 및 툴팁 클래스 예외 처리, 네이티브 Enter 보조 발송 조건 개선
- `preload-ai-webview.js`: Gemini 입력창/전송 버튼 셀렉터 및 툴팁 필터링 동기화
- `app.js`: 업데이트 내역 목록 내 현재 버전(`v5.4.1`) 뱃지 인식 동기화
- `index.html`: 업데이트 내역 및 환영 모달 버전 배지 `v5.4.1` 반영
- `ai-start-page.html`: 푸터 버전 `v5.4.1` 동기화
- `package.json`, `package-lock.json`, `README.md`: 앱 버전 `v5.4.1` 판올림
- `history/update-5.4.1-20261104.md`: v5.4.1 릴리즈 노트 신규 생성
