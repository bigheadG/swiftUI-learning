
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
        }.background(.red)
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
        .background(.yellow)
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
  
    <img width="249" alt="image" src="https://github.com/user-attachments/assets/e02637c1-aea6-48de-8c6b-fa2d0cd153a9">

2. 按下子視圖的「增加」按鈕一次：

    <img width="259" alt="image" src="https://github.com/user-attachments/assets/c2eb28d3-29eb-40a8-a58d-f5aa951a9ad4">

3. 再次按下子視圖的「增加」按鈕：
   
   <img width="266" alt="image" src="https://github.com/user-attachments/assets/2a6afe4a-6576-4814-8245-588eb949fdce">

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

---

## UIView (UIKit) vs View (SwiftUI) vs Struct

### UIView (UIKit)
- `UIView` 是 UIKit 中的核心類，繼承自 `NSObject`，是 **引用類型（class）**。
- 基於 **命令式編程**，需要手動管理 UI 的更新和狀態。
- **特性**：
  - 支援複雜的動畫和高效能渲染。
  - 需要處理佈局約束（Auto Layout）或 Frame-based 佈局。
  - 使用引用語義，適合在多個地方共享同一物件。
- **典型場景**：
  - 用於開發 iOS 早期應用或需要對底層渲染有精確控制的場景。
  
### View (SwiftUI)
- `View` 是 SwiftUI 中的核心協議，遵循該協議的類型通常是 **值類型（struct）**。
- 基於 **聲明式編程**，由狀態驅動 UI 的自動更新。
- **特性**：
  - 更簡潔的語法，適合快速開發現代化的 UI。
  - 依賴狀態管理（如 `@State`、`@Binding`）來同步更新 UI。
  - 單向資料流：狀態變化會觸發視圖的重新生成。
- **典型場景**：
  - 現代 iOS 開發，強調與資料狀態同步的簡化 UI 建構。

### Struct
- `struct` 是 Swift 中的一種值類型，主要用於表示不可變的數據結構。
- **特性**：
  - 值語義：結構的副本不會影響原始數據。
  - 分配在棧中，比類型的堆分配更高效。
  - 適合用於封裝無需共享的數據或視圖。
- 在 SwiftUI 中，大多數 `View` 是以 `struct` 定義的，因為值語義能更好地實現不可變性和性能優化。

### 對比

| 特性                  | UIView (UIKit)       | View (SwiftUI)       | Struct               |
|-----------------------|----------------------|----------------------|----------------------|
| **類型**              | class（引用類型）    | struct（值類型）     | struct（值類型）     |
| **編程風格**          | 命令式              | 聲明式               | N/A                  |
| **狀態更新方式**      | 手動更新            | 狀態驅動             | N/A                  |
| **內存管理**          | 堆分配              | 堆或棧分配           | 棧分配               |
| **適用場景**          | 複雜動畫與舊式應用  | 現代應用快速開發      | 數據封裝與視圖結構   |
| **可變性**            | 支援修改            | 依賴狀態重建         | 通常不可變（除非 `var` 定義） |


---

## 使用多個 @Binding 和 View 更新機制

### 核心規則
1. **單一數據驅動更新**  
   SwiftUI 是基於單一數據流的架構。當某個 `@Binding` 的值改變時，SwiftUI 會檢測到變化並重新生成依賴這個數據的視圖樹。其他與該數據無關的部分不會受到影響。

2. **依賴變化觸發**  
   - 如果多個 `@Binding` 的值發生變化且這些值都影響同一個視圖，這個視圖會重新生成。
   - 如果某個 `@Binding` 的值改變，而這個值沒有被用到，SwiftUI 不會重新計算這個視圖。

### 範例 1：多個 `@Binding` 影響同一視圖

```swift
struct ParentView: View {
    @State private var valueA = 0
    @State private var valueB = 0

    var body: some View {
        ChildView(valueA: $valueA, valueB: $valueB)
    }
}

struct ChildView: View {
    @Binding var valueA: Int
    @Binding var valueB: Int

    var body: some View {
        VStack {
            Text("Value A: \(valueA)")
            Text("Value B: \(valueB)")
        }
    }
}
```

#### 當 `valueA` 或 `valueB` 改變時：
- **結果**：`ChildView` 的 `body` 被重新計算，因為 `body` 中使用了 `valueA` 和 `valueB`。

---

### 範例 2：多個 `@Binding`，部分屬性未使用

```swift
struct ParentView: View {
    @State private var valueA = 0
    @State private var valueB = 0

    var body: some View {
        ChildView(valueA: $valueA, valueB: $valueB)
    }
}

struct ChildView: View {
    @Binding var valueA: Int
    @Binding var valueB: Int

    var body: some View {
        VStack {
            Text("Value A: \(valueA)") // 只使用了 valueA
        }
    }
}
```

#### 當 `valueA` 改變時：
- **結果**：`ChildView` 的 `body` 被重新計算，因為 `valueA` 被使用。

#### 當 `valueB` 改變時：
- **結果**：`ChildView` 的 `body` 不會重新計算，因為 `valueB` 沒有在 `body` 中被使用。

---

### 範例 3：多層傳遞的影響

```swift
struct ParentView: View {
    @State private var valueA = 0
    @State private var valueB = 0

    var body: some View {
        VStack {
            ChildView(valueA: $valueA, valueB: $valueB)
            Text("Parent Value A: \(valueA)")
        }
    }
}

struct ChildView: View {
    @Binding var valueA: Int
    @Binding var valueB: Int

    var body: some View {
        VStack {
            Text("Child Value A: \(valueA)")
            Text("Child Value B: \(valueB)")
        }
    }
}
```

#### 當 `valueA` 改變時：
- **結果**：
  - `ChildView` 和 `ParentView` 的 `body` 都會被重新計算，因為它們都依賴於 `valueA`。
  - `Text("Parent Value A: \(valueA)")` 和 `Text("Child Value A: \(valueA)")` 都會更新。

#### 當 `valueB` 改變時：
- **結果**：
  - 只有 `ChildView` 的 `body` 被重新計算，因為只有它依賴於 `valueB`。

---

### 總結

1. **單一數據觸發更新**  
   每個 `@Binding` 的改變只會影響使用該數據的視圖部分。

2. **性能優化**  
   SwiftUI 的視圖更新是 **增量的**，只會重新計算與變更數據相關的部分，無關部分不會受到影響。

3. **多個 `@Binding` 交互**  
   如果多個 `@Binding` 同時改變且都被視圖使用，SwiftUI 會重新計算這些部分。未改變或未使用的 `@Binding` 不會影響視圖更新。

4. **設計建議**  
   - 避免在 `@Binding` 中傳遞未使用的數據，減少不必要的依賴。
   - 盡量將狀態拆分為更小的單元，保持清晰的數據流向。

