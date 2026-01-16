# 🎯 빌드 전 체크리스트

## ✅ Unity 설정 (Firebase 임포트 불필요!)

**중요: Unity에는 Firebase SDK를 설치할 필요가 없습니다!**  
Firebase는 JavaScript에서만 사용됩니다.

- [ ] **GameObject 설정**
  - [ ] 침대 오브젝트 생성
  - [ ] 이름을 `Bed`로 변경 (대소문자 정확히)
  - [ ] Collider2D 컴포넌트 추가 (Box Collider 2D 또는 Circle Collider 2D)
  - [ ] `CircularMovement.cs` 스크립트 추가

- [ ] **WebGL 빌드 설정**
  - [ ] File → Build Settings
  - [ ] Platform: **WebGL** 선택
  - [ ] "Switch Platform" 클릭 (처음이라면)
  - [ ] Scenes In Build에 현재 씬 추가
  // 테스트

---

## 🔥 Firebase 설정 (5분)

자세한 설명: [FIREBASE_QUICK_SETUP.md](file:///c:/Users/JJW/PlanitHideout/WebApp/FIREBASE_QUICK_SETUP.md)

- [ ] **1. Firebase 프로젝트 생성**
  - [ ] https://console.firebase.google.com/ 접속
  - [ ] "프로젝트 추가" 클릭
  - [ ] 프로젝트 이름 입력 (예: `room-decorator`)
  - [ ] Google Analytics: 선택 안 함
  - [ ] "프로젝트 만들기" 클릭

- [ ] **2. 웹 앱 등록**
  - [ ] 프로젝트 개요 → 웹 아이콘(`</>`) 클릭
  - [ ] 앱 닉네임 입력
  - [ ] **Firebase SDK 구성 복사**
  - [ ] `index.html` 파일 열기 (171번째 줄 근처)
  - [ ] `firebaseConfig` 값을 복사한 실제 값으로 교체

- [ ] **3. Firestore Database 활성화**
  - [ ] 빌드 → Firestore Database
  - [ ] "데이터베이스 만들기" 클릭
  - [ ] **테스트 모드로 시작** 선택
  - [ ] 위치: **asia-northeast3 (Seoul)**

- [ ] **4. Authentication 활성화**
  - [ ] 빌드 → Authentication
  - [ ] "시작하기" 클릭
  - [ ] 로그인 방법 → **익명** 활성화

---

## 🚀 빌드 및 테스트

- [ ] **Unity WebGL 빌드**
  - [ ] Unity: File → Build Settings → Build
  - [ ] 폴더 선택: `c:\Users\JJW\PlanitHideout\WebApp\Build`
  - [ ] 빌드 완료 대기 (5-10분)

- [ ] **빌드 파일 확인**
  ```
  WebApp/
  ├─ Build/
  │   ├─ Build.data
  │   ├─ Build.framework.js
  │   ├─ Build.loader.js  ← 이게 있어야 함!
  │   └─ Build.wasm
  └─ index.html
  ```

- [ ] **로컬 서버 실행**
  ```powershell
  cd c:\Users\JJW\PlanitHideout\WebApp
  python -m http.server 8000
  ```

- [ ] **브라우저 테스트**
  - [ ] http://localhost:8000 접속
  - [ ] F12 (개발자 도구) 열기
  - [ ] Console에 "Firebase 로그인 성공" 메시지 확인
  - [ ] "방 입장하기" 버튼 클릭
  - [ ] Unity가 로드되는지 확인
  - [ ] 침대 클릭해서 방향 전환 테스트
  - [ ] Console에 "회전 방향 변경" 메시지 확인
  - [ ] 로비로 돌아갔다가 다시 입장
  - [ ] 이전 회전 방향이 유지되는지 확인

---

## ❓ 자주 묻는 질문

### Q1: Unity에 Firebase SDK를 설치해야 하나요?
**A: 아니요!** Firebase는 JavaScript(index.html)에서만 사용합니다.  
Unity는 JavaScript와 `DllImport`로 통신합니다.

### Q2: Node.js나 npm을 설치해야 하나요?
**A: 아니요!** 로컬 테스트는 Python HTTP 서버만으로 가능합니다.  
하이브리드 앱 패키징 시에만 Cordova/Capacitor가 필요합니다.

### Q3: 빌드 시간이 얼마나 걸리나요?
**A:** 프로젝트 크기에 따라 5-15분 정도 소요됩니다.  
첫 WebGL 빌드는 더 오래 걸릴 수 있습니다.

### Q4: 테스트 모드 보안 규칙이 만료되면?
**A:** 30일 후 만료됩니다. `firestore.rules` 파일의 내용을  
Firebase Console → Firestore → 규칙 탭에 붙여넣으세요.

---

## 🆘 문제 해결

### Unity 로드 실패
- `file://`로 직접 열면 안 되고, 반드시 HTTP 서버 사용
- `Build/Build.loader.js` 파일이 존재하는지 확인

### Firebase 연결 실패
- `firebaseConfig` 값이 올바르게 붙여넣어졌는지 확인
- Firestore와 Authentication이 활성화되었는지 확인

### 침대 클릭이 안 됨
- GameObject 이름이 정확히 `Bed`인지 확인
- Collider2D가 추가되어 있는지 확인
- Unity Console에 에러가 없는지 확인

### 상태가 저장 안 됨
- F12 Console에 에러가 있는지 확인
- Firebase Console → Firestore → 데이터 탭에서 직접 확인
- `users/{userId}/roomSettings/main` 문서가 생성되었는지 확인

---

## 📚 참고 문서

- [FIREBASE_QUICK_SETUP.md](file:///c:/Users/JJW/PlanitHideout/WebApp/FIREBASE_QUICK_SETUP.md) - Firebase 단계별 설정
- [SETUP_GUIDE.md](file:///c:/Users/JJW/PlanitHideout/WebApp/SETUP_GUIDE.md) - 전체 시스템 가이드
- [implementation_plan.md](file:///C:/Users/JJW/.gemini/antigravity/brain/359163de-d188-4bd7-b7eb-e76ba765b011/implementation_plan.md) - 구현 계획서
