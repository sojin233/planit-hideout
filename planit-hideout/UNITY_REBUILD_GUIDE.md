# Unity WebGL 빌드 - 최종 가이드

## ✅ Build 폴더 삭제 완료!

이전 Brotli 파일이 모두 삭제되었습니다.

---

## 🚀 Unity에서 다시 빌드

### 1️⃣ Player Settings 확인

```
Unity Editor:
1. Edit → Project Settings
2. Player 탭 클릭
3. 왼쪽에서 WebGL 아이콘 클릭
4. Publishing Settings 확장
5. Compression Format 확인:
   
   ✅ Disabled (압축 없음 - 가장 확실!)
   또는
   ✅ Gzip (압축 사용)
   
   ❌ Brotli (이거면 안 됨!)
```

### 2️⃣ 빌드 실행

```
1. File → Build Settings
2. Platform: WebGL 선택됨 확인
3. Build 버튼 클릭
4. 폴더 선택:
   c:\Users\JJW\PlanitHideout\WebApp\Build
   (Build 폴더는 자동 생성됨)
5. 빌드 시작 (5-10분 소요)
```

### 3️⃣ 빌드 완료 확인

```
WebApp/Build/ 폴더에 생성되는 파일:

Disabled 압축:
├─ Build.data
├─ Build.framework.js       ← .js 파일
├─ Build.loader.js
└─ Build.wasm                ← .wasm 파일

Gzip 압축:
├─ Build.data.gz
├─ Build.framework.js.gz     ← .gz 파일
├─ Build.loader.js
└─ Build.wasm.gz             ← .gz 파일

❌ .br 파일이 있으면 안 됨!
```

---

## 🌐 서버 재시작 & 테스트

### CMD 창:

```cmd
# 1. 서버가 실행 중이면 Ctrl + C로 중지

# 2. 서버 재시작
npx http-server -p 8000

# 3. 성공 메시지 확인:
# Starting up http-server, serving ./
# Available on: http://127.0.0.1:8000
```

### 브라우저:

```
1. Chrome/Edge 열기
2. 주소: http://localhost:8000
3. Ctrl + Shift + R (강력 새로고침)
4. "방 입장하기" 버튼 클릭
5. Unity 로드 확인!
```

---

## 🐛 여전히 에러가 나면?

### Compression Format을 Disabled로!

가장 확실한 방법:
```
Unity:
Edit → Project Settings → Player → WebGL
→ Publishing Settings
→ Compression Format: Disabled
→ 다시 빌드
```

압축 없이 빌드하면 **100% 작동**합니다!

---

## ✅ 체크리스트

- [x] Build 폴더 삭제
- [ ] Unity Player Settings → Compression Format: Disabled (또는 Gzip)
- [ ] File → Build Settings → Build
- [ ] 빌드 완료 (5-10분)
- [ ] CMD에서 서버 재시작
- [ ] 브라우저 새로고침 (Ctrl + Shift + R)

---

**지금 Unity에서 빌드 시작하세요! Compression Format을 Disabled로 하면 가장 확실합니다!** 🚀
