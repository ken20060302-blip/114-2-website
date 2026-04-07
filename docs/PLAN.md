# 第一步計畫：做出輸入與顯示功能

## 目標
做出一個可以輸入 GitHub 名字、並顯示出來（加上大頭貼）的簡單畫面。

---

## 步驟

### 步驟 1：加入輸入框和按鈕
在 `App.vue` 的「畫面」區加一個**輸入框**和一個**按鈕**。

### 步驟 2：建立「盒子」
在「腳本區」建立三個「盒子」：
- `nameBox`：用來存放使用者輸入的名字
- `errorBox`：用來存放錯誤訊息
- `profileBox`：用來存放查到的個人資料

### 步驟 3：連接輸入框
用 `v-model` 把輸入框和 `nameBox` 連起來，這樣**打在輸入框的字，會自動存進盒子裡**。

### 步驟 4：寫下「查名字」動作
當按下按鈕時，觸發「查名字」這個動作：
1. 打電話給 GitHub API（`fetch`）
2. 如果名字不存在，捕獲錯誤
3. 如果成功，把資料裝進 `profileBox`

### 步驟 5：顯示結果
用「顯示開關」(`v-if`) 控制：
- 錯誤訊息：只有當 `errorBox` 不是空的時候才顯示
- 個人資料：只有當 `profileBox` 有資料的時候才顯示

---

## 用到的程式碼

### 建立盒子
```javascript
const nameBox = ref('')
const errorBox = ref('')
const profileBox = ref(null)
```

### 負責「記住」名字的那一行
```javascript
const nameBox = ref('')
```

### 控制「什麼時候該出現」的那一行
```vue
<div v-if="errorBox !== ''">查無此人</div>
```

---

## 實作後修改

### 讓照片變圓
在「化妝」區加上這一行：
```css
.avatar {
  border-radius: 50%;
}
```

### 加上深色背景
在「化妝」區加上：
```css
.container {
  background-color: #1a1a2e;
}
```
