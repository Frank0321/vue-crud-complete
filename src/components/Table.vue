<template>
  <div>
    <table class="table" cellspacing="0" cellpadding="0">
      <thead>
      <tr>
        <th align="left">No.</th>
        <th align="left">Name</th>
        <th align="left">Phone</th>
      </tr>
      </thead>
      <tbody>
      <!-- 需要設定好 key -->
      <tr v-for="(item, index) in tableData" :key="index">
        <td>{{ index+1 }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.phone }}</td>
        <td>
          <!-- 取出來之後，透過 emit 傳給 父元件 -->
          <button class="edit-btn" @click="editItem(item)">Edit</button>
          <button class="delete-btn" @click="deleteItem(item)">Delete</button>
        </td>
      </tr>
      </tbody>
    </table>
<!--    <p>共 {{ tableData.length }} 筆</p>-->
    <p>共 {{totalLength}} 筆</p>
  </div>
</template>

<script>
export default {
  name: "Table",
  //從外面傳進來 父->子
  props:{
    tableData:{
      type: Array,
      //如果 default 是 array 或是 物件 的話，要透過 fun 的方式來 return
      default:()=>{
        return [];
      },
    }
    //tableData 這樣設定，較容易找 debug
  },
  //要按照他的生命週期
  //資料和畫面都渲染完成
  methods:{
    editItem(item){
      this.$emit('edit',item);
    },
    deleteItem(item){
      this.$emit('delete',item);
    },
  },

  mounted() {
    console.log('tableData', this.tableData);
  },
  computed:{
    totalLength (){
      let length = this.tableData.length;
      console.log("length", length);
      return length;
    },
  }
}
</script>

<style scoped>

</style>