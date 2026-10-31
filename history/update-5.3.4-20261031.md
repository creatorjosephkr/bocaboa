# BocaBoa 업데이트 내역 - v5.3.4

- **업데이트 일자**: 2026년 10월 31일
- **대상 버전**: v5.3.4
- **저장소**: [creatorjosephkr/bocaboa](https://github.com/creatorjosephkr/bocaboa)

---

## 🚀 주요 변경 사항

### 1. AI 로스팅 가이드 모드 '페르소나 및 지시사항 수정' 모달 클릭 시 웹뷰 뒤로 숨김 버그 해결
- **드롭다운 외부 클릭 이벤트 오작동 및 BrowserView 강제 복원 문제 해결**:
  - AI 가이드 창 상단의 내비게이션 드롭다운 메뉴 외부 클릭 핸들러(`document.addEventListener('click')`)가 페르소나 수정 모달 내부를 클릭할 때도 실행되어 `closeNavDropdown()`을 호출하던 문제를 수정했습니다.
  - 드롭다운이 열려 있지 않음에도 `closeNavDropdown()`이 무조건 Electron 네이티브 뷰인 `BrowserView`를 다시 표시(`setBrowserViewVisibility(true)`)하도록 동작하여, macOS에서 네이티브 계층이 HTML 모달 창을 덮어 가리던 현상을 원천 해결했습니다.
- **모달 활성 상태 통합 검사 (`isAnyModalOpen()`) 및 가드 로직 도입**:
  - 페르소나 모달, 깃허브 세션 모달, 데이터 전송 창, 알림/확인 팝업 등 어떤 모달 창이라도 열려 있는 상태에서는 `showBrowserView()`가 호출되더라도 `BrowserView`를 다시 표시하지 않도록 차단 가드를 적용했습니다.
  - 내비게이션 드롭다운 외부 클릭 리스너 역시 드롭다운 메뉴가 실제로 열려 있는 상태(`navDropdownMenu.classList.contains('open')`)에서만 닫기 동작이 수행되도록 수정했습니다.
- **모달 내부 클릭 이벤트 버블링 차단 (`stopPropagation`)**:
  - `.modal-content`, `.alert-popup`, `.fav-popup`, `.claude-modal` 등 모달 본문 영역을 클릭할 때 이벤트가 상위 `document`로 전파되지 않도록 차단하여, 텍스트 에어리어나 버튼 클릭 시 의도치 않은 전역 이벤트가 트리거되는 것을 방지했습니다.
- **모달 오픈 시 대화 화면 스냅샷 보존**:
  - 페르소나 수정 모달이 열릴 때 기존 대화창 화면을 캡처 스냅샷 이미지로 배경에 유지한 채 `BrowserView`를 숨기도록(`await hideBrowserView()`) 개선하여 배경이 깜빡이거나 비어 보이지 않도록 완성도를 높였습니다.

### 2. 페르소나 수정 모달 단축키 및 포커스 편의성 향상
- **ESC 키 닫기 지원**:
  - 페르소나 수정 모달이 열려 있을 때 `ESC` 키를 누르면 안전하게 모달이 닫히고 `BrowserView`가 복원됩니다.
- **모달 활성화 중 로스팅 단축키 릴레이 및 포커스 튐 방지**:
  - 모달 창이 열려 있는 동안에는 키 입력이 메인 창의 로스팅 단축키로 릴레이되거나 메인 창으로 포커스가 강제 전환되는 동작을 차단하여, 지시사항 텍스트 작성 중 타이핑이 끊기거나 의도치 않은 로스팅 이벤트가 실행되지 않도록 보호했습니다.

---

### 📦 변경된 파일
- `ai-webview.html`: 모달 활성 검사(`isAnyModalOpen()`), BrowserView 표시/숨김 가드 로직(`hideBrowserView()`, `showBrowserView()`), 드롭다운 외부 클릭 검사 강화, 모달 내부 클릭 전파 차단, ESC 닫기 및 단축키 릴레이 차단
- `index.html`: 업데이트 내역 및 환영 모달 버전 배지 `v5.3.4` 반영
- `app.js`: 업데이트 내역 목록의 현재 버전 표시(`v5.3.4`) 동기화
- `ai-start-page.html`: 푸터 버전 `v5.3.4` 반영
- `package.json`, `package-lock.json`, `README.md`: 앱 버전 `v5.3.4` 업데이트
- `history/update-5.3.4-20261031.md`: v5.3.4 릴리즈 노트 신규 생성
