# 🔥 Firebase 권한 오류 해결 가이드

## ❌ 에러 메시지
```
FirebaseError: Missing or insufficient permissions
```

**원인:** Firestore Database 보안 규칙이 익명 사용자의 읽기/쓰기를 막고 있음

---

## ✅ 해결 방법 (5분)

### 1️⃣ **Firebase Console 접속**
🔗 https://console.firebase.google.com/

### 2️⃣ **프로젝트 선택**
- `planithideout` 프로젝트 클릭

### 3️⃣ **Firestore Database → 규칙**
```
왼쪽 메뉴: 빌드 → Firestore Database
상단 탭: 규칙 (Rules)
```

### 4️⃣ **보안 규칙 수정**

**현재 규칙 (만료됨):**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.time < timestamp.date(2026, 2, 14);
    }
  }
}
```

**새 규칙 (테스트용 - 30일):**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.time < timestamp.date(2026, 3, 15);
    }
  }
}
```

**또는 영구 (익명 사용자 허용):**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // 익명 로그인 사용자 허용
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 5️⃣ **게시 버튼 클릭**
```
오른쪽 상단: "게시" 또는 "Publish" 클릭
```

---

## 🎯 추천 규칙 (프로덕션)

개발 완료 후:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // 사용자는 자신의 데이터만 읽기/쓰기
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      
      match /furniture/{furnitureId} {
        allow read, write: if request.auth != null && request.auth.uid == userId;
      }
    }
  }
}
```

---

## 📋 단계별 스크린샷 가이드

### 1. Firebase Console
```
https://console.firebase.google.com/
→ planithideout 프로젝트 선택
```

### 2. Firestore Database
```
왼쪽 메뉴: 빌드
→ Firestore Database 클릭
```

### 3. 규칙 탭
```
상단 탭: 데이터 | 규칙 | 인덱스 | 사용량
→ "규칙" 클릭
```

### 4. 규칙 수정
```
에디터에 새 규칙 복사 → 붙여넣기
→ 우측 상단 "게시" 버튼 클릭
```

---

## ✅ 완료 확인

브라우저 새로고침 후:

**F12 Console:**
```
✅ Bed 위치 저장: (x, y)
📥 Bed 위치 로드: (x, y)
```

**❌ 에러 없음!**

---

## ⏰ 테스트 모드 만료 시

30일 후 다시 권한 오류 발생하면:

1. Firebase Console → Firestore → 규칙
2. `timestamp.date(2026, 3, 15)` → 새 날짜로 변경
3. 게시

또는 영구 규칙으로 변경 (위의 "익명 사용자 허용" 규칙)

---

**지금 바로 Firebase Console에서 규칙을 수정하세요!** 🔥
