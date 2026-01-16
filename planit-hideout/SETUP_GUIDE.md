# Unity WebGL + Firebase 통합 가이드

## 📦 Unity 설정

### 1. GameObject 설정
1. **Hierarchy**에서 침대 오브젝트 이름을 `Bed`로 설정 (JavaScript에서 참조)
2. `Bed`에 **Collider2D** 컴포넌트 추가 (클릭 감지용)
   - Box Collider 2D 또는 Circle Collider 2D 추가
   - `Is Trigger`는 체크하지 않음
3. `CircularMovement.cs` 스크립트를 `Bed`에 추가

### 2. WebGL 빌드 설정

#### Build Settings (File → Build Settings)
- **Platform**: WebGL 선택 후 "Switch Platform"
- **Scenes In Build**: 현재 씬 추가
- **Build 폴더**: `WebApp/Build`로 설정

#### Player Settings (Edit → Project Settings → Player)
```yaml
Company Name: YourCompany
Product Name: RoomDecorator
Version: 1.0

WebGL Settings:
  ├─ Resolution and Presentation
  │   ├─ Default Canvas Width: 1280
  │   ├─ Default Canvas Height: 720
  │   └─ WebGL Template: Minimal
  │
  ├─ Publishing Settings
  │   ├─ Compression Format: Brotli (권장)
  │   ├─ Data Caching: ✓
  │   ├─ Enable Exceptions: Explicitly Thrown Exceptions Only
  │   └─ Code Optimization: Runtime Speed
  │
  └─ Other Settings
      └─ Strip Engine Code: ✓ (파일 크기 감소)
```

### 3. 빌드 실행

```powershell
# Unity Editor에서:
# File → Build Settings → Build
# 빌드 폴더: c:\Users\JJW\PlanitHideout\WebApp\Build
```

빌드 완료 후 생성되는 파일:
```
WebApp/
├─ Build/
│   ├─ Build.data          # 게임 데이터
│   ├─ Build.framework.js  # Unity 프레임워크
│   ├─ Build.loader.js     # 로더 스크립트
│   └─ Build.wasm          # WebAssembly 바이너리
├─ index.html              # 메인 HTML
└─ StreamingAssets/        # (있다면) 추가 리소스
```

---

## 🔥 Firebase 설정

### 1. Firebase 프로젝트 생성
1. [Firebase Console](https://console.firebase.google.com/) 접속
2. "프로젝트 추가" 클릭
3. 프로젝트 이름: `room-decorator` (예시)
4. Google Analytics: 선택 사항
5. "프로젝트 만들기" 클릭

### 2. Firebase 웹 앱 등록
1. 프로젝트 개요 → ⚙️ 설정 → 프로젝트 설정
2. "앱 추가" → **웹** 아이콘 클릭
3. 앱 닉네임: `Room Decorator Web App`
4. Firebase Hosting 설정: 선택 사항
5. **Firebase SDK 구성** 복사:

```javascript
// index.html의 firebaseConfig에 붙여넣기
const firebaseConfig = {
    apiKey: "복사한_API_KEY",
    authDomain: "프로젝트ID.firebaseapp.com",
    projectId: "프로젝트ID",
    storageBucket: "프로젝트ID.appspot.com",
    messagingSenderId: "복사한_ID",
    appId: "복사한_APP_ID"
};
```

### 3. Firestore Database 활성화
1. Firebase Console → **Firestore Database**
2. "데이터베이스 만들기" 클릭
3. **테스트 모드로 시작** (개발용)
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;  // ⚠️ 프로덕션에서는 변경 필요!
       }
     }
   }
   ```
4. 위치: asia-northeast3 (서울)

### 4. Authentication 활성화
1. Firebase Console → **Authentication**
2. "시작하기" 클릭
3. **로그인 방법** 탭 → "익명" 활성화
   - 익명 로그인 토글 ON
   - "저장" 클릭

---

## 🚀 로컬 테스트

### 방법 1: Live Server (Visual Studio Code)
```bash
# VS Code 확장 프로그램 설치
# "Live Server" by Ritwick Dey

# index.html 우클릭 → "Open with Live Server"
```

### 방법 2: Python HTTP Server
```powershell
# WebApp 폴더로 이동
cd c:\Users\JJW\PlanitHideout\WebApp

# Python 3가 설치되어 있다면
python -m http.server 8000

# 브라우저에서 http://localhost:8000 접속
```

### 방법 3: Node.js HTTP Server
```powershell
# npx 사용 (Node.js 설치 필요)
cd c:\Users\JJW\PlanitHideout\WebApp
npx http-server -p 8000
```

---

## 🔍 통신 흐름 확인

### Unity → JavaScript (상태 저장)
```
1. 사용자가 침대 클릭
   ↓
2. CircularMovement.OnMouseDown() 실행
   ↓
3. ToggleDirection() → SaveRotationDirection(bool)
   ↓
4. JavaScript의 SaveRotationDirection() 호출
   ↓
5. Firebase Firestore에 저장
```

### JavaScript → Unity (상태 로드)
```
1. Unity 로드 시 RequestInitialState() 실행
   ↓
2. JavaScript의 LoadRotationDirection() 호출
   ↓
3. Firebase에서 현재 상태 조회
   ↓
4. unityInstance.SendMessage('Bed', 'SetRotationDirection', value)
   ↓
5. Unity의 SetRotationDirection(int) 실행
```

---

## 🐛 디버깅 팁

### Chrome DevTools 콘솔 확인
```javascript
// Unity → JS 통신 확인
"Unity → JS: 회전 방향 변경 true"

// JS → Unity 통신 확인
"JS → Unity: 초기 방향 전송 1"

// Firebase 저장 확인
"Firebase에 설정 저장: { clockwise: true, ... }"
```

### Unity Console 확인
```
회전 방향 변경: 시계방향
Firebase에서 상태 로드 완료: 반시계방향
```

### Firebase Console 확인
1. Firestore Database → 데이터 탭
2. `users/{userId}/roomSettings/main` 문서 확인
3. `clockwise` 필드 값 확인

---

## 📱 하이브리드 앱 패키징

### Cordova 사용 예시
```powershell
# Cordova 설치
npm install -g cordova

# 프로젝트 생성
cordova create RoomDecoratorApp com.yourcompany.roomdecorator "Room Decorator"
cd RoomDecoratorApp

# WebApp 파일들을 www/ 폴더로 복사
# (index.html, Build/ 폴더 등)

# 플랫폼 추가
cordova platform add android
cordova platform add ios

# 빌드
cordova build android
```

### Capacitor 사용 예시  
```powershell
npm install @capacitor/core @capacitor/cli
npx cap init "Room Decorator" com.yourcompany.roomdecorator

# WebApp 폴더를 웹 디렉토리로 설정 (capacitor.config.ts)
npx cap add android
npx cap add ios
npx cap sync
```

---

## ⚠️ 주의사항

1. **GameObject 이름**: JavaScript에서 `SendMessage('Bed', ...)`로 호출하므로 반드시 `Bed`로 설정
2. **Collider 필수**: `OnMouseDown()` 작동을 위해 Collider 컴포넌트 필요
3. **빌드 경로**: `index.html`에서 `Build/Build.loader.js` 경로가 정확해야 함
4. **Firebase 보안**: 프로덕션 배포 시 Firestore 규칙을 반드시 수정
5. **CORS 오류**: 로컬 파일(`file://`)로는 작동 안 함, 반드시 HTTP 서버 사용

---

## 📋 체크리스트

- [ ] Unity에 침대 GameObject 생성 (이름: `Bed`)
- [ ] Collider2D 컴포넌트 추가
- [ ] CircularMovement.cs 스크립트 추가
- [ ] WebGL로 빌드 (Build 폴더에 생성)
- [ ] Firebase 프로젝트 생성
- [ ] Firestore Database 활성화
- [ ] Authentication (익명 로그인) 활성화
- [ ] index.html에 Firebase 설정 입력
- [ ] 로컬 HTTP 서버로 테스트
- [ ] Chrome DevTools에서 통신 로그 확인
- [ ] Firebase Console에서 데이터 저장 확인
