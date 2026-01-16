# Firebase 빠른 설정 가이드 (5분 완료)

## 1단계: Firebase 프로젝트 생성

### 1-1. Console 접속
🔗 https://console.firebase.google.com/

### 1-2. 프로젝트 만들기
1. "프로젝트 추가" 버튼 클릭
2. **프로젝트 이름**: `room-decorator` (원하는 이름)
3. Google Analytics: **선택 안 함** (체크 해제) → 빠른 설정
4. "프로젝트 만들기" 클릭 → 약 30초 대기

---

## 2단계: 웹 앱 등록

### 2-1. 앱 추가
1. 프로젝트 개요 화면에서 **"웹"** 아이콘(`</>`) 클릭
2. **앱 닉네임**: `Room Decorator Web`
3. Firebase Hosting 설정: **체크 안 함**
4. "앱 등록" 버튼 클릭

### 2-2. Firebase SDK 구성 복사
아래와 같은 코드가 표시됩니다:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "room-decorator-xxxxx.firebaseapp.com",
  projectId: "room-decorator-xxxxx",
  storageBucket: "room-decorator-xxxxx.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:xxxxxxxxxxxxxx"
};
```

**이 내용을 복사**하고 → **계속** 버튼 클릭

### 2-3. index.html에 붙여넣기
`c:\Users\JJW\PlanitHideout\WebApp\index.html` 파일을 열고:

**171번째 줄** 근처에 있는 `firebaseConfig`를 **복사한 내용으로 교체**:

```javascript
// ❌ 기존 (교체할 부분)
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    // ...
};

// ✅ 새로운 (복사한 실제 값으로)
const firebaseConfig = {
    apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXX",  // 실제 값
    authDomain: "room-decorator-xxxxx.firebaseapp.com",
    // ... (복사한 나머지)
};
```

---

## 3단계: Firestore Database 활성화

### 3-1. Firestore 만들기
1. Firebase Console 왼쪽 메뉴 → **빌드** → **Firestore Database**
2. "데이터베이스 만들기" 버튼 클릭

### 3-2. 보안 규칙 선택
- **테스트 모드로 시작** 선택 (개발용)
- "다음" 클릭

> ⚠️ 테스트 모드는 30일 후 만료됩니다. 나중에 프로덕션 규칙으로 변경하세요.

### 3-3. Cloud Firestore 위치
- **asia-northeast3 (Seoul)** 선택
- "사용 설정" 클릭 → 약 1분 대기

✅ 완료되면 빈 데이터베이스 화면이 표시됩니다.

---

## 4단계: Authentication 활성화

### 4-1. 인증 설정
1. Firebase Console 왼쪽 메뉴 → **빌드** → **Authentication**
2. "시작하기" 버튼 클릭

### 4-2. 익명 로그인 활성화
1. **로그인 방법** 탭 클릭
2. **익명** 항목 찾기
3. 클릭 → 토글 스위치 **사용 설정**으로 변경
4. "저장" 버튼 클릭

✅ "익명" 상태가 **"사용 설정됨"**으로 표시되어야 합니다.

---

## ✅ 설정 완료!

Firebase 설정이 모두 끝났습니다. 이제 Unity를 빌드하고 테스트할 수 있습니다.

---

## 🚀 Unity 빌드 (다음 단계)

### 1. Unity에서 빌드
```
1. Unity Editor 열기
2. File → Build Settings
3. Platform: WebGL 선택 → "Switch Platform" (처음이라면)
4. "Build" 버튼 클릭
5. 폴더 선택: c:\Users\JJW\PlanitHideout\WebApp\Build
   (폴더 새로 만들기: Build)
6. 빌드 시작 (5-10분 소요)
```

### 2. 로컬 테스트
```powershell
# WebApp 폴더로 이동
cd c:\Users\JJW\PlanitHideout\WebApp

# Python HTTP 서버 실행
python -m http.server 8000
```

### 3. 브라우저 테스트
- 🌐 http://localhost:8000 접속
- F12 (개발자 도구) → Console 탭에서 로그 확인
- "방 입장하기" 버튼 클릭
- Unity 씬이 로드되면 침대 클릭해서 방향 전환 테스트

---

## 🐛 문제 해결

### Firebase 연결 실패
- Console에 "Firebase 로그인 성공: [user-id]" 출력되는지 확인
- `firebaseConfig` 값이 올바르게 붙여넣어졌는지 확인

### Unity 로드 실패
- `Build/Build.loader.js` 파일이 존재하는지 확인
- 반드시 HTTP 서버로 실행 (파일 직접 열기 불가)

### 침대 클릭이 안 됨
- Unity에서 GameObject 이름이 `Bed`인지 확인
- Collider2D가 추가되어 있는지 확인
