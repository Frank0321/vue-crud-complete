<template>
  <div id="contact">
    <!-- Header component -->
    <Header :pageTitle="pageTitle" />
    <div class="container">
      <!-- 按鈕要觸發事件 -->
      <button class="default-btn" v-on:click="addItem">Add</button>
      <!-- Please create a Table component and complete CRUD operations. -->
      <!-- 放置匯入的 Table 這一個 component -->
      <!-- 利用 v-bind 透過 props 把資料傳進去 table -->
<!--      <Table v-bind:table-data="tableData"/>-->
      <!-- 透過簡寫，可以省略 v-bind ，直接綁定-->
      <Table :table-data="tableData" @edit="editItem" @delete="deleteItem"/>

    </div>
    <!-- Modal component -->
    <!-- Use the `v-if` directive to conditionally render a block. -->
    <!-- https://vuejs.org/v2/guide/conditional.html -->
    <!-- 新增視窗，主要為彈出介面 -->            <!--子元件的事件，由父元件的方法控制 -->
    <Modal :title="modal.title" v-if="modal.show" @close="closeModal" >
      <!-- 會顯示在 slot 裡面 -->
      <!-- 對應後面的 slot 名稱 -->
<!--      <template v-slot:abc>-->
      <label>Name</label>
      <!-- 雙向綁定資料 -->
      <input v-model="editData.name"/>
      <label>Phone</label>
      <input v-model="editData.phone"/>
      <button class="default-btn float-right" @click="saveItem">Save</button>
    </Modal>
  </div>
</template>
<script>
// Import components
import Header from "../components/Header.vue";
import Modal from "../components/Modal.vue";
import Table from "../components/Table.vue";
// Import axios
// https://github.com/axios/axios
import axios from "axios"

export default {
  name: "Contact",
  // Defined components
  // https://vuejs.org/v2/guide/components.html
  //當 import 一個 components 進來，要定義在這邊
  components: { Header, Modal, Table },
  data() {
    return {
      pageTitle: "Contacts.",
      //和新增有關的物件
      modal: {
        title: "",
        show: false,
      },
      //要傳入的東西
      // 此為測試的假資料
      // tableData:[{
      //   no:0,
      //   name:"Frank",
      //   phone: "0987654321",
      // }],
      tableData:[],
      editData:{
        name:"",
        phone:""
      },
      mockdata:[
        {
          name:"default",
          phone:"default"
        }
      ]
    };
  },
  created: function(){
    this.fetchData();
  },
  //負責管理頁面上的 fun
  methods:{
    fetchData(){
      let self = this;
      axios
          //https://www.npmjs.com/package/json-server
          //json-server --watch db.json
          .get('http://localhost:3000/contact')   //放 api 的網址
          .then(function (response) {
            // handle success
            console.log("response", response);

            self.tableData = response.data;
          })
          .catch(function (error) {
            // handle error
            console.log("not connect to db service, please open json service");
            console.log(error);
            self.tableData = self.mockdata;
          })
          .then(function () {
            // always executed
          });
    },
    mounted() {
      this.fetchData();
    },
    closeModal(arg){
      if(arg){
        console.log("arg", arg);
      }
      this.modal.show = false;
    },
    //新增時要做的事情
    addItem(){
      this.modal={
        title: "新增",
        show: true
      }
      this.editData={}
    },
    editItem(item) {
      this.editData = item;
      this.modal ={
        title: "編輯",
        show: true
      }
    },
    async deleteItem(item) {
      await axios
          .delete(`http://localhost:3000/contact/${item.id}`)   //放 api 的網址
          .then(function (response) {
            // handle success
            console.log(response);
            // self.tableData = response.data;
            self.fetchData();
            // self.modal.show = false;
          })
          .catch(function (error) {
            console.log(error);
          })
          .then(function () {
          });
      await this.fetchData();
    },

    async saveItem(){
      if(this.editData.id){
        console.log(this.editData.id);
        let self = this;
        await axios
            //因為新增
            .put(`http://localhost:3000/contact/${this.editData.id}`
                , this.editData)   //放 api 的網址
            .then(function (response) {
              // handle success
              console.log(response);
              // self.tableData = response.data;
              self.fetchData();
              // self.modal.show = false;
            })
            .catch(function (error) {
              console.log(error);
            })
            .then(function () {
            });
      }else {
        //先產生一個 id
        this.editData.id = this.tableData.length + 1;
        console.log("editData", this.editData);
        //fun 裡面沒有 this
        let self = this;
        //呼叫 api
        await axios
            //因為新增
            .post('http://localhost:3000/contact', this.editData)   //放 api 的網址
            .then(function (response) {
              // handle success
              console.log(response);
              // self.tableData = response.data;
              self.fetchData();

            })
            .catch(function (error) {
              // handle error
              console.log(error);
            })
            .then(function () {
              // always executed
            });
      }
      this.closeModal();
    }
  }
};
</script>

<style>
@import "../assets/css/colors.scss";
@import "../assets/css/main.scss";

.container {
  padding:  30px 200px;
}
p {
  color: gray;
}
input {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
table {
  width: 100%;
  display: table;


}
td {
  height: 50px;
  background: white;
}
.edit-btn , .delete-btn{
  background: #59a1f5;
  color: green;
  padding: 8px 20px;
  font-size: 15px;
  font-weight: 800;
}
.delete-btn {
  background: coral;
  color: darkred;
  margin: 10px;
}


</style>
