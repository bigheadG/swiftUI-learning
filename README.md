
# 計數器應用程式

這是一個使用 SwiftUI 開發的計數器範例，展示如何使用 `@State` 和 `@Binding` 管理與傳遞狀態，實現動態更新的用戶介面。

---

## 功能

- 使用 `@State` 管理本地狀態，隨值的改變自動更新 UI。
- 使用 `@Binding` 在父子視圖之間共享與同步狀態。
- 簡單互動：按下按鈕增加計數器數值，狀態同步顯示於父子視圖。

---

## 程式碼概覽

### 主視圖（Parent View）

```swift
import SwiftUI

struct ParentView: View {
    @State private var counter = 0

    var body: some View {
        VStack {
            Text("主視圖計數器：\(counter)")
                .font(.title)
                .padding()
            ChildView(counter: $counter) // 傳遞 @Binding 給子視圖
                .padding()
        }
    }
}
```

### 子視圖（Child View）

```swift
import SwiftUI

struct ChildView: View {
    @Binding var counter: Int // 綁定父視圖的狀態

    var body: some View {
        VStack {
            Text("子視圖計數器：\(counter)")
                .font(.headline)
                .padding()
            Button("增加") {
                counter += 1 // 修改綁定的值，父視圖的狀態同步更新
            }
            .padding()
        }
    }
}
```

---

## 運作原理

### 1. `@State`
- `@State` 是用於本地狀態管理的屬性包裝器。
- 主視圖（`ParentView`）中的 `counter` 使用 `@State` 宣告，當值改變時，SwiftUI 自動重新計算主視圖的 `body` 並更新畫面。

### 2. `@Binding`
- `@Binding` 是用於將父視圖的狀態傳遞給子視圖的屬性包裝器。
- 子視圖（`ChildView`）中的 `@Binding var counter` 綁定到父視圖的 `counter`。
- 透過 `@Binding`，子視圖可以直接修改父視圖的狀態。

---

## 畫面預覽

### 主視圖與子視圖的互動
1. 初始畫面：
   ```
   主視圖計數器：0
   子視圖計數器：0
   [增加按鈕]
   ```

2. 按下子視圖的「增加」按鈕一次：
   ```
   主視圖計數器：1
   子視圖計數器：1
   [增加按鈕]
   ```

3. 再次按下子視圖的「增加」按鈕：
   ```
   主視圖計數器：2
   子視圖計數器：2
   [增加按鈕]
   ```

---

## 關鍵概念

### 1. 單一資料來源
- 主視圖的 `@State` 是唯一的資料來源，子視圖通過 `@Binding` 與其共享相同的狀態。
- 確保資料來源集中，避免狀態不一致問題。

### 2. @State 和 @Binding 的互動
- `@State`：用於本地管理狀態，值改變時觸發視圖重新計算。
- `@Binding`：用於從父視圖傳遞狀態，實現父子視圖之間的同步更新。

---

## 需求
- Xcode 12 或更新版本
- Swift 5.3 或更新版本
- iOS 14 或更新版本

---

## 快速開始

1. 複製此程式碼或從版本庫中克隆專案。
2. 使用 Xcode 開啟專案。
3. 執行應用程式，測試主視圖與子視圖的互動。

---

## 自訂功能

- 增加「減少」按鈕來實現計數器的遞減功能。
- 使用 `@EnvironmentObject` 在多層視圖中共享全域狀態。
- 添加動畫效果，使 UI 更生動。

---

## 範例擴展

### 增加「減少」功能
修改 `ChildView`，新增減少按鈕：

```swift
Button("減少") {
    counter -= 1
}
.padding()
```

---

## 授權

此專案為開源，並依照 [MIT 授權條款](LICENSE) 提供使用。

---
