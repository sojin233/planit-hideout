# 🏠 방꾸미기 WebGL 테스트

Unity WebGL 기반 방꾸미기 앱입니다. 가구를 드래그로 배치하고 Firebase에 자동 저장됩니다.

---

## 🚀 빠른 실행 (3단계)

### 1️⃣ Node.js 설치
🔗 https://nodejs.org/ → **LTS 버전** 다운로드 & 설치

### 2️⃣ 서버 실행
```bash
# 압축 해제 후 WebApp 폴더에서 CMD/PowerShell 열기
# (폴더 주소창에 "cmd" 입력 후 Enter)

# 서버 실행 (처음 실행 시 설치 확인 → y 입력)
npx http-server -p 8000
```

**성공 메시지:**
```
Starting up http-server, serving ./
Available on: http://127.0.0.1:8000
```

### 3️⃣ 브라우저 접속
```
http://localhost:8000
```

**Chrome, Edge, Firefox 권장**

---

## 🎮 사용 방법

1. 브라우저 접속 후 Unity 로딩 대기 (10-20초)
2. **Bed** 드래그: 마우스/터치로 이동
3. **Chair** 드래그: 마우스/터치로 이동
4. **자동 저장**: 위치가 Firebase에 저장됨
5. **새로고침**: 이전 위치로 복원됨

---

## 📁 폴더 구조

```
WebApp/
├─ index.html          메인 HTML (Firebase 포함)
├─ UnityBuild/         Unity WebGL 빌드
│   └─ Build/
│       ├─ UnityBuild.data
│       ├─ UnityBuild.framework.js
│       └─ ...
└─ README.md          이 파일
```

---

## 🐛 문제 해결

### "서버가 시작 안 됨"
**Node.js 설치 확인:**
```bash
node --version
# v20.x.x 이상 표시되어야 함
```

### "Unity 로딩이 멈춤"
**F12 → Console 탭 확인:**
- 빨간 에러 메시지 확인
- `Ctrl + Shift + R` (강력 새로고침)

### "가구가 안 움직임"
**EventSystem 확인:**
- Unity가 정상 로드되었는지 확인
- 브라우저 새로고침

### "저장이 안 됨"
**Firebase 상태 확인:**
- F12 → Console에서 `✅ Firebase 로그인 성공` 메시지 확인
- 저장 메시지 확인: `✅ Bed 위치 저장: (x, y)`

---

## 💡 개발자 도구 (선택사항)

**F12 키:**
- **Console**: 로그 확인
- **Network**: 로딩 상태 확인

**유용한 로그:**
```
✅ Firebase 로그인 성공: [userId]
✅ Unity 로드 완료
✅ Bed 가구 등록 완료
✅ Bed 위치 저장: (1.50, 2.30)
📥 Bed 위치 로드: (1.50, 2.30)
```

---

## ⚠️ 주의사항

- `file://` 직접 열기 ❌ (HTTP 서버 필수!)
- 인터넷 연결 필요 (Firebase)
- 모바일/태블릿도 터치 지원

---

## 🔄 서버 종료

CMD/PowerShell 창에서:
```
Ctrl + C
```

---

**문제 발생 시 개발자에게 문의하세요!**

