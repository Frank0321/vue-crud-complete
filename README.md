# vue : 實作CRUD 範例檔案

此次內容包含 :

- 利用 Cli 建置 vue 專案
- vue 的相關介紹
- vue 實作 CRUD



## 環境相關建置

- 環境建置

  - start

    ```
    npm run serve
    ```

    

- JSON Server

  - Install JSON Server

    ```
    npm install -g json-server
    ```

  - Create a `db.json` file with some data

    ```\
    {
      "contact": [
        {
          "name": "Blairsss",
          "phone": "0912345678",
          "id": 1
        }
    }
    ```

  - Start JSON Server

    ```
    json-server --watch db.json
    ```

    then will create Json DB like :

    ```
    http://localhost:3000/contact
    ```

  - [ref](https://www.npmjs.com/package/json-server)



- axios 安裝

  - Using npm

    ```
    npm install axios
    ```

  - Performing a `GET` request

    ```
    // Make a request for a user with a given ID
    axios.get('/user?ID=12345')
      .then(function (response) {
        // handle success
        console.log(response);
      })
      .catch(function (error) {
        // handle error
        console.log(error);
      })
      .then(function () {
        // always executed
      });
    ```

  - [ref](https://github.com/axios/axios)





## vue 的相關介紹

本專案為 Cli 所建置的 vue 專案，實際上會與 gridsome 建置專案有所落差，這邊先以 Cli 建置專案來介紹

- 資料結構





## vue 實作 CRUD

- 主要檔案 : 

  - Contact : 最主要顯示的頁面來源
  - Table : 資料表格的 components
  - Modal : 新增或是編輯的跳出視窗
  - Header : 頁面的 title
  - db.json : 資料庫

- API 套件

  - 官方推薦 [Axios](https://axios-http.com/) 這個 API 套件
  - 參閱環境建置進行安裝
  - 在 component 中使用 Axios 請記得 import : `import axios from "axios"`
  - 本次使用 [JSON Server](https://github.com/typicode/json-server) 作為虛擬 server
  - 參閱環境建置進行安裝
  - Start JSON Server : `json-server --watch db.json`

  ---

  ### Read 資料說明 :

  在 Contact 使用 Table 這一個 component ，並將資料由 Contact 傳至 Table 進行顯示 (父 - > 子，props)

  - Table 顯示的部分 :

    ```
    <!-- 需要設定好 key -->
    <tr v-for="(item, index) in tableData" :key="index">
      <td>{{ index+1 }}</td>
      <td>{{ item.name }}</td>
      <td>{{ item.phone }}</td>
      <td>
    ```

    - tableData 預計會由 Contact 將資料傳入
    - for loop 被要求說要有一個 key 值

    

  - Table 資料的部分 :

    ```
    //從外面傳進來 父->子
      props:{
        tableData:{
          type: Array,
          //如果 default 是 array 或是 物件 的話，要透過 fun 的方式來 return
          default:()=>{
            return [];
          },
        }
    ```

    - props : 接收外部資料的屬性

    - 將資料定義好，到時候容易找 buge

      

  - Contact 使用 Table 物件的部分

    ```
    <Table v-bind:tableData="tableData"/>
    ```

    - 放置匯入的 Table 這一個 component
    - 利用 v-bind 透過 props 把資料傳進去 table
    - v-bind : tableData，可以用 : tableData 代替，如下 : 
    
    ```
    <Table :table-data="tableData" />
    ```
    
    
    
  - import component 的部分

    需要一個 components 裝起來

    ```
    components: { Header, Modal, Table }
    ```

    

- Contact 的 data

  新增一個 tableData 作為存放 data 的部分

  

- Contact 匯入資料 db.json 的資料

  ```
  fetchData(){
      let self = this;
      axios
      //https://www.npmjs.com/package/json-server
      //json-server --watch db.json
      .get('http://localhost:3000/contact')   //放 api 的網址
      .then(function (response) {
      // handle success
      console.log(response);
      self.tableData = response.data;
      })
      .catch(function (error) {
      // handle error
      console.log(error);
      })
      .then(function () {
      // always executed
      });
  ```

  - 使用到 axios 這一個 api

  - 回傳的 reponse 內的 data 才是我們要的資料

  - 內層 function 無法使用到 this 物件，需要先定義 

    ```
    let self = this
    ```

    才能將資料在往內層 function 帶

    

- 拿資料放在 mounted 這一個生命週期內 :

  - mounted : vue 初始化掛載完成

  - 因此會把 fetchData() 在 mounted 內進行呼叫

    ```
    mounted() {
        this.fetchData();
        },
    ```

  

- Table 新增 computed 計算總共筆數

  ```
  computed:{
    totalLength (){
      let length = this.tableData.length;
      console.log("length", length);
      return length;
    },
  }
  ```

  - 已經取回 tableData 了，並計算結果



### Create 資料操作說明 :

在 Contact 使用 Modal 這一個 components，顯示在 Contact 畫面上，但關閉視窗的功能 (在 Modal 上) 要在 Contact 上進行關閉 (子的功能傳到父的介面去操作 ，$emit)

- Modal 的 close :

  ```
  <button class="close-btn" @click="closeModal">&times;</button>
  ```

  ```
  closeModal(){
    //子元件的事件傳給父元件 子 -> 父
    this.$emit("close", "ABCD")
  }
  ```

  - 在 method 中，使用 $emit 的方式傳給父元件，同時也可以傳遞參數
  - 而在父元件要使用此功能時，需要 @close，然後對應到自己的方法

  ```
  <Modal :title="modal.title" v-if="modal.show" @close="closeModal" >
  ```

  ```
  closeModal(arg){
    if(arg){
      console.log("arg", arg);
    }
    this.modal.show = false;
  },
  ```

  - modal.show 為控制 Modal 這個元件的顯示與否

    

- Modal 的 slot

  ```
  <slot />
  ```

  - 為一個佔位符號，可再由父元件，將需要的內容塞進去

  ```
  <Modal :title="modal.title" v-if="modal.show" @close="closeModal" >
  .....塞到這裡
  </Modal>
  ```

  - 如果 slot 有標註 name 的話，只能塞到對應的名稱內，否則無法顯示

    ```
    <slot name="abc"/>
    ```

    只能在有標示 abc 內容，才會顯示

    ```
    <template v-slot:abc>
    .....塞到這裡
    </template>
    ```

- Contact 儲存資料

  - save-btn 是由 Contact 產生的，由 Contact 定義 saveItem 就可以了

    ```
    <button class="default-btn float-right" @click="saveItem">Save</button>
    ```

    ```
    saveItem(){
    ....
    }
    ```

    會再與Update 一起說明

    


### Update 資料說明 :

   

  

### delete 資料說明 :

  
