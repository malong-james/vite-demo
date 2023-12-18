<template>
  <div class="table-box">
    <!--标题-->
    <div class="title">
      <h2>最简单的CRUDdemo</h2>
    </div>
    <!--query -->
    <div class="query-box">
      <el-input class="query-input" v-model="queryInput" placeholder="请输入姓名搜索" />
      <el-button type="primary" @click="handleAdd">增加</el-button>
    </div>
    <!-- table -->
    <el-table 
    ref="multipleTableRef"
    :data="tableData"
    style="width: 100%"
    @selection-change="handleSelectionChange"
    border>

    <el-table-column type="selection" width="55" />
    <el-table-column prop="name" label="姓名" width="120" />
    <el-table-column prop="email" label="邮箱" width="120" />
    <el-table-column prop="phone" label="电话" width="120" />
    <el-table-column prop="state" label="状态" width="120" />
    <el-table-column prop="address" label="地址" width="300" />
    <el-table-column fixed="right" label="操作" width="120">
      <template #default="scope">
        <el-button link type="primary" size="small" @click="handleRowDel(scope.row)" style="color: #F56C6C;">删除</el-button
        >
        <el-button link type="primary" size="small" @click="handleRowUpdate(scope.row)" style="color: #409EFF;">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>
  <!--dialog -->

  <el-dialog v-model="dialogFormVisible" :title="dialogType === 'add'?'新增':'编辑'">
    <el-form :model="tableForm">
      <el-form-item label="姓名" :label-width="80">
        <el-input v-model="tableForm.name" autocomplete="off" />
      </el-form-item>
      <el-form-item label="邮箱" :label-width="80">
        <el-input v-model="tableForm.email" autocomplete="off" />
      </el-form-item>
      <el-form-item label="电话" :label-width="80">
        <el-input v-model="tableForm.phone" autocomplete="off" />
      </el-form-item>
      <el-form-item label="状态" :label-width="80">
        <el-input v-model="tableForm.state" autocomplete="off" />
      </el-form-item>
      <el-form-item label="地址" :label-width="80">
        <el-input v-model="tableForm.address" autocomplete="off" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="dialogConfirm">
          确认
        </el-button>
      </span>
    </template>
  </el-dialog>

  </div>
</template>

<script setup>
  import {ref} from 'vue'
  /*数据 */
  let queryInput = ref('')
  let tableData = ref([  
    {
    id :'1',
    date: '2016-05-03',
    name: 'Tom',
    email: 'eee@qq.com',
    phone: '12333333333',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },{
    id :'2',
    date: '2016-05-03',
    name: 'jack',
    email: 'eee@qq.com',
    phone: '12333333333',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },{
    id :'3',
    date: '2016-05-03',
    name: 'oleg',
    email: 'eee@qq.com',
    phone: '12333333333',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },{
    id :'4',
    date: '2016-05-03',
    name: 'max',
    email: 'eee@qq.com',
    phone: '12333333333',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },]);

  let multipleSelection = ref([])
  let dialogFormVisible = ref(false)
  let tableForm = ref({
    name:'张胜男',
    email:'ddd@qq.com',
    phone:'13333333',
    state:'在职',
    address:'花姑娘河路'

  })
  let dialogType = ref('add')
  let currIndex = ref('0')
  /*方法*/
  const handleRowClick = () =>{
    console.log('click')
  }
  const handleSelectionChange = (val) => {
  multipleSelection.value = val
  console.log(val);
  }
  const handleAdd = () => {
    dialogFormVisible.value = true;
    tableForm.value = {}
    dialogType = ref('add')
  }

  const dialogConfirm = ()=>{
    dialogFormVisible.value = false;
    //1 拿到数据
    //2 添加到table

    
    if(dialogType.value === 'add'){
      tableData.value.push({
      id: (tableData.value.length +  1 ).toString(),
      ...tableForm.value
    })
    }else{
      let index = tableData.value.findIndex(item => item.id == currIndex)
      console.log(index)

    
      tableData.value[index] =  tableForm.value
    }
     
   console.log(tableData.value)
  }

  const handleRowDel = ({id}) => {
      console.log(id)
      let index = tableData.value.findIndex(item => item.id == id)
      console.log(index)
      tableData.value.splice(index,1)
  }

  const handleRowUpdate = ({id}) => {
    console.log(id)
    dialogFormVisible.value = true;
    let index = tableData.value.findIndex(item => item.id == id)

    tableForm.value = tableData.value[index]
    dialogType = ref('update')
    currIndex = id
  }




</script>


<style scoped>
.table-box{
  margin: 200px auto;
  width: 800px;
}
.title{
  text-align: center;
}
.query-box{
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
.query-input{
  width: 200px;
}
</style>
