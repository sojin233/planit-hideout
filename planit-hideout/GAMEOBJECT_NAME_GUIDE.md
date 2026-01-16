# GameObject 이름이 중요한 이유

## ❓ "GameObject 이름을 Bed로 안 하면 안 되나요?"

**답: GameObject 이름도 "Bed"여야 합니다!**

---

## 🔍 이유: `SendMessage()` 때문

### JavaScript → Unity 통신 방식

```javascript
// index.html (JavaScript)
unityInstance.SendMessage('Bed', 'SetFurniturePosition', jsonData);
                          ↑
                    GameObject 이름으로 찾음!
```

Unity는 **GameObject 이름**으로 오브젝트를 찾아서 함수를 호출합니다.

---

## 📋 설정 방법

### Unity Hierarchy:
```
Scene
└─ Bed  ← GameObject 이름 (F2로 변경)
    └─ FurnitureController (Script)
        └─ Furniture Id: "Bed"  ← Inspector 설정
```

**둘 다 같아야 합니다!**

---

## ✅ 올바른 설정

| GameObject 이름 | Furniture Id (Inspector) | 결과 |
|---|---|---|
| **Bed** | **Bed** | ✅ 작동! |
| Bed | Chair | ❌ 로드 실패 (ID 불일치) |
| BedObject | Bed | ❌ SendMessage 실패 |

---

## 🔧 지금 확인하세요

Unity에서:

1. **Hierarchy**에서 GameObject 선택
2. **F2** 키로 이름 변경 → `Bed`
3. **Inspector**에서 FurnitureController 확인
4. **Furniture Id**: `Bed`

**둘 다 정확히 "Bed"여야 합니다!**

---

## 💡 나중에 여러 가구 추가할 때

```
Hierarchy:
├─ Bed (GameObject)
│   └─ FurnitureController
│       └─ Furniture Id: "Bed"
├─ Chair (GameObject)
│   └─ FurnitureController
│       └─ Furniture Id: "Chair"
└─ Desk (GameObject)
    └─ FurnitureController
        └─ Furniture Id: "Desk"
```

**GameObject 이름 = Furniture Id** 규칙을 지키세요!
