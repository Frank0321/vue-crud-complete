# vue 元件實體的 生命週期

- 生命週期的圖示 :

  ![image](https://vuejs.org/images/lifecycle.png)

- Vue.js 提供了開發者在這些週期階段做對應處理的 callback function，這些 callback function 就被稱呼為 `生命週期的 Hooks function`

- Hooks function 為圖中紅色的部分 :

  - beforeCreate : Vue 實體被建立，狀態與事件都尚未初始化

  - created : Vue 實體已建立，狀態與事件已初始化完成

    ​				(`prop`、`data`、`computed` 等屬性已建立，`vm$el` 屬性無法使用 )

  - beforeMount : Vue 實體尚未與模板 (DOM 節點) 綁定

  - mounted : Vue 實體與掛載完成，`el` 的目標 DOM 被 `＄el` 所替換 (可以視作 jQuery 的 ready)

  - beforeUpdate : 當狀態被變動時，畫面同步更新前

  - updated : 當狀態被變動時，畫面已同步更新完成

  - beforeDestroy : Vue 實體物件被銷毀前

  - destroyed : Vue 實體物件被銷毀完畢

- [參考](https://book.vue.tw/CH1/1-7-lifecycle.html)



- vue 生命週期的三個階段

  ![image](C:\Softleader\FrankGit\vue-crud-complete\ref\pic\lifecycle 三個階段.png)

  

## beforeCreate

- 新增 vue 實體時，就會執行的 function
- 很少會使用，因為連 data、event 都還沒有



## created

- 較常使用
- 已經觀察到 Data 內有那些屬性



## beforeMount

- 會先檢查有沒有 "el" 這個東西，
  如果沒有，則會用 vm.$mount(el) 指定給某一個 element
  如果也沒有的 vm.$moumt(el) 則會被瀏覽器回收掉
- 檢查有沒有 "template"
  有的話，compile template 到 render function 裡面
  沒有的話，則會把 html 當作是 template 進行編譯



## Mounted

- 建立 el， 並把內容作結合
- 載入完成



## beforeUpdate

- 資料與畫面還沒同步時



## updateed

- 資料與畫面同步更新



## beforeDestroy

- 



## Destroy

- 











