
# SwiftUI 列表範例

這是一個使用 SwiftUI 建立和管理列表的範例，展示了如何創建基本的列表、添加刪除功能以及自定義刪除按鈕文字。

---

## **功能**

- 顯示一組靜態的水果名稱列表。
- 支持在列表中刪除項目。
- 自定義刪除按鈕的文字和行為。

---

## **SwiftUI 基本列表範例**

以下是一個簡單的 SwiftUI 列表範例，展示如何使用 `List` 組件來顯示數據：

```swift
import SwiftUI

struct ContentView: View {
    // 資料來源：靜態數據列表
    let items = ["蘋果", "香蕉", "橘子", "葡萄", "草莓"]

    var body: some View {
        NavigationView {
            List(items, id: \.self) { item in
                NavigationLink(destination: DetailView(selectedItem: item)) {
                    Text(item)
                }
            }
            .navigationTitle("水果清單")
        }
    }
}

struct DetailView: View {
    // 顯示選中的水果名稱
    var selectedItem: String

    var body: some View {
        VStack {
            Text("您選擇了：")
            Text(selectedItem)
                .font(.largeTitle)
                .padding()
        }
        .navigationTitle("詳細資訊")
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

### **功能描述**
- 使用 `NavigationView` 提供導航，讓用戶點擊每個項目進入詳細頁面。
- `List` 用於顯示靜態的水果名稱。
- 點擊每個水果名稱會跳轉到詳細頁面，顯示所選項目的詳細信息。

---

## **添加刪除功能的列表**

下面的範例展示如何在列表中添加刪除功能，使用戶可以刪除項目：

```swift
import SwiftUI

struct ContentView: View {
    @State private var items = ["蘋果", "香蕉", "橘子", "葡萄", "草莓"]

    var body: some View {
        NavigationView {
            List {
                ForEach(items, id: \.self) { item in
                    Text(item)
                }
                .onDelete(perform: deleteItem) // 添加刪除功能
            }
            .navigationTitle("水果清單")
            .toolbar {
                EditButton() // 添加右上角的編輯按鈕
            }
        }
    }

    // 刪除項目的方法
    func deleteItem(at offsets: IndexSet) {
        items.remove(atOffsets: offsets)
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

### **刪除功能描述**
- **`@State` 屬性**：列表的數據使用 `@State` 進行管理，以支持動態更新。
- **`onDelete(perform:)`**：使用此修飾符為列表添加刪除功能。
- **`EditButton()`**：添加到工具欄中的編輯按鈕，允許用戶進入編輯模式，從而刪除列表項目。

---

## **自定義刪除按鈕文字**

你還可以自定義列表的刪除按鈕文字和行為，以下是一個使用 `.swipeActions` 來實現自定義刪除的範例：

```swift
import SwiftUI

struct ContentView: View {
    @State private var items = ["蘋果", "香蕉", "橘子", "葡萄", "草莓"]

    var body: some View {
        NavigationView {
            List {
                ForEach(items, id: \.self) { item in
                    Text(item)
                        .swipeActions {
                            Button(role: .destructive) {
                                deleteItem(item: item)
                            } label: {
                                Text("移除") // 自定義刪除按鈕的文字
                            }
                        }
                }
            }
            .navigationTitle("水果清單")
        }
    }

    // 自定義刪除項目的方法
    func deleteItem(item: String) {
        if let index = items.firstIndex(of: item) {
            items.remove(at: index)
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

### **自定義刪除描述**
- **`swipeActions` 修飾符**：自定義每個列表項目在向左滑動時顯示的操作按鈕。
- **自定義刪除按鈕**：
  - `Button(role: .destructive)`：設置按鈕為 "破壞性" 行為，如刪除操作，按鈕會顯示為紅色。
  - 自定義按鈕文字為 "移除" 來取代默認的 "Delete"。

---

## **結論**

這些範例展示了 SwiftUI 列表的基本使用，以及如何添加和自定義刪除功能。你可以根據自己的應用需求進一步擴展這些功能，例如添加新項目、修改項目或使用不同的樣式來呈現列表。

希望這些範例對你有幫助，並能夠讓你更好地理解 SwiftUI 中的列表操作！

## **要求**
- Xcode 12 或更高版本
- Swift 5.3 或更高版本
- iOS 14 或更高版本

## **授權**

此專案為開源，並依照 [MIT 授權條款](LICENSE) 提供使用。

