---
publish: true
---

# 나만의 PKM 앱 기술 스택

---

- **스택**: 웹 기술
  - CSS + TSX 사용
  - Monorepo 구조의 공유 코어 중심
- **프레임워크**:
  - **데스크톱**: Electron + React/Vite
  - **모바일**: Capacitor + React/Vite
  - **웹사이트**: Next.js
- **라이브러리**:
  - **Markdown AST**: Remark
  - **WYSIWYG 에디터**: CodeMirror 6
  - **UI - 에디터 래퍼**: React-CodeMirror
  - **CRDT 실시간 동기화**: Yjs
  - **중앙 서버 경유 데이터 동기화**: Y-Websocket
  - **에디터 - 동기화 래퍼**: Y-Codemirror.next
  - **인덱스 및 캐시 DB**: Better-SQLite3
  - **동기화  - DB 래퍼**: Y-SQLite
  - **Server Fetch 캐싱 및 비동기 무효화**: TanSatck Query
  - **고성능 렌더링 엔진**: TanStack Virtual (react-virtual)
  - **샌드박스 플러그인 지원**: QuickJS
  - **스타일링 시스템**: Tailwind CSS
- **내장 기능**:
  - WYSIWYG 실시간 미리보기
  - CRDT 실시간 동기화
