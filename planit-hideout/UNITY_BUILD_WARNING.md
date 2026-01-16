# ⚠️ Unity 빌드 시 주의사항

## 🚨 문제: Unity가 index.html을 계속 덮어씀!

Unity WebGL 빌드 시 `index.html`을 자동 생성하여 **우리가 만든 Firebase 코드를 덮어씁니다**.

---

## ✅ 해결 방법

### **옵션 1: 빌드 후 index.html 복원 (현재 방식)**

1. Unity 빌드 완료
2. **즉시 index.html 다시 저장** (VS Code에서 Ctrl+S)
3. 서버 재시작

**장점:** 간단  
**단점:** 빌드할 때마다 반복 필요

### **옵션 2: Build 폴더를 다른 곳에 (권장)**

Unity 빌드를 `WebApp/UnityBuild/`에 하고, `index.html`은 `WebApp/`에:

```
WebApp/
├─ index.html          ← 우리 파일 (Firebase 포함)
├─ UnityBuild/         ← Unity가 빌드하는 곳
│   ├─ Build/
│   ├─ TemplateData/
│   └─ index.html      ← Unity 기본 (사용 안 함)
```

그 다음 우리 `index.html`의 경로 수정:
```javascript
var buildUrl = "UnityBuild/Build";  // 경로 변경
```

---

## 🎯 지금 상황

**Unity가 index.html을 덮어써서 Firebase 함수가 사라졌습니다.**

**해결:**
1. 방금 index.html을 다시 복원했습니다
2. **브라우저 새로고침**: Ctrl + Shift + R
3. F12 Console 확인: `SaveFurniturePosition` 함수 정의됨 확인

---

## 📋 앞으로 할 일

**매번 빌드 후:**
1. Unity 빌드 완료
2. VS Code에서 `index.html` 열기
3. **Ctrl + Z** (되돌리기) → Firebase 코드 복원
4. **Ctrl + S** (저장)
5. 서버 재시작 + 브라우저 새로고침

또는

**Unity Build Settings 변경:** (더 나은 방법)
1. Build 폴더를 `WebApp/UnityBuild/`로 변경
2. `index.html`의 경로만 수정
3. 더 이상 덮어쓰기 없음!

---

**지금은 브라우저 새로고침만 하면 작동합니다!**
