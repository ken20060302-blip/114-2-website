<script setup>
import { ref } from 'vue'

// 建立一個「盒子」用來存放使用者輸入的名字
const nameBox = ref('')

// 建立一個「盒子」用來存放錯誤訊息
const errorBox = ref('')

// 建立一個「盒子」用來存放查到的個人資料
const profileBox = ref(null)

// 按下按鈕時要做的事
function 查名字() {
  // 如果名字是空的，就跳出警告
  if (nameBox.value === '') {
    alert('請輸入 GitHub 名字！')
    return
  }

  // 每次查詢前，先把錯誤盒子清空
  errorBox.value = ''
  profileBox.value = null

  // 開始打電話給 GitHub API
  fetch(`https://api.github.com/users/${nameBox.value}`)
    .then(response => {
      // 如果電話打不通（名字不存在），就報錯
      if (!response.ok) {
        throw new Error('查無此人')
      }
      return response.json()
    })
    .then(data => {
      // 成功拿到資料，把資料裝進盒子裡
      profileBox.value = data
    })
    .catch(error => {
      // 發生錯誤了，把錯誤訊息裝進錯誤盒子
      errorBox.value = error.message
    })
}
</script>

<template>
  <!-- 這裡放「畫面」- 決定網頁長什麼樣子 -->
  <div class="container">
    <h1>GitHub 偵察機</h1>

    <!-- 輸入框：打在這裡的字會自動存進 nameBox -->
    <input 
      v-model="nameBox" 
      type="text" 
      placeholder="輸入 GitHub 名字..."
    />

    <!-- 按鈕：按下就會觸發 查名字() 這個動作 -->
    <button @click="查名字">搜尋</button>

    <!-- 只有當 errorBox 不是空的時候，才顯示這行字 -->
    <div v-if="errorBox !== ''" class="error">
      {{ errorBox }}
    </div>

    <!-- 只有當 profileBox 有資料的時候，才顯示這區塊 -->
    <div v-if="profileBox" class="profile">
      <!-- 大頭貼：border-radius: 50% 讓方形的圖片變圓 -->
      <img 
        :src="profileBox.avatar_url" 
        alt="大頭貼" 
        class="avatar"
      />
      <h2>{{ profileBox.login }}</h2>
      <p>{{ profileBox.bio || '這個人很懶，沒有自我介紹' }}</p>
    </div>
  </div>
</template>

<style scoped>
/* 這裡放「化妝」- 決定顏色、大小、位置等 */
.container {
  max-width: 400px;
  margin: 50px auto;
  padding: 30px;
  background-color: #1a1a2e;  /* 深色背景 */
  color: white;
  border-radius: 16px;
  text-align: center;
}

input {
  padding: 10px;
  font-size: 16px;
  border-radius: 8px;
  border: none;
  width: 60%;
  margin-right: 10px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 8px;
  border: none;
  background-color: #e94560;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #d63651;
}

.error {
  margin-top: 20px;
  color: #ff6b6b;
  font-weight: bold;
}

.profile {
  margin-top: 30px;
}

/* 這一行讓照片變成圓形！ */
.avatar {
  width: 150px;
  height: 150px;
  border-radius: 50%;  /* 50% = 圓形 */
  border: 4px solid #e94560;
}
</style>
