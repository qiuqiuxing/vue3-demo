<template>
  <div style="padding:10px">
    <el-input style="width: 300px" placeholder="请输入名称" v-model="name" clearable></el-input>
    <el-button type="primary" @click="search" style="margin-left: 5px" >查询数据</el-button>
    <el-button type="primary" @click="handleAdd">新增数据</el-button>

    <div>
      <el-table :data="tableData" style="padding: 10px 0">
        <el-table-column prop="date" label="日期" width="180" align="center"/>
        <el-table-column prop="name" label="姓名" width="180" align="center"/>
        <el-table-column prop="address" label="地址" align="center"/>
        <el-table-column label="操作">
          <template #default="scope">   <!--           用scope来取出行对象   scope.row，scope$index哪一行-->
            <el-button link type="primary" size="small" @click="handleEdit(scope.row,scope.$index)">编辑</el-button>
            <!--            .prevent阻止事件冒泡-->
            <el-button link type="danger" size="small" @click.prevent="remove(scope.$index)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <el-dialog v-model="dialogFormVisible" title="信息" width="40%">
      <el-form :model="form" label-width="100px" style="padding-right: 30px">
        <el-form-item label="日期">
          <el-input v-model="form.date" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="姓名">
          <el-input v-model="form.name" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="地址">
          <el-input v-model="form.address" autocomplete="off"/>
        </el-form-item>
      </el-form>
      <template #footer>
        <div class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取消</el-button>
          <el-button type="primary" @click="save">确认</el-button>
        </div>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import {reactive, ref} from 'vue'
// 不会被代理对象所困扰
// const state =reactive({
//   tableData:[
//     {
//       date: '2016-05-11',
//       name: 'Jerry',
//       address: 'No. 189, Grove St, Los Angeles',
//     },
//     {
//       date: '2016-05-02',
//       name: 'Tom',
//       address: 'No. 189, Grove St, Los Angeles',
//     }],
//   form:{}
// })

let tableData = ref([
  {
    date: '2016-05-11',
    name: 'Jerry',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    date: '2016-05-02',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
  }])
//const声明为常量，常量是不能被修改的
const dialogFormVisible = ref(false)
//let声明为变量，变量时可以被修改的
let form = reactive({})
const globalIndex = ref(-1)//全局保存的编辑的行号
const name = ref('')   //如果要对name进行改变的话必须用ref进行包装  ref是包装简单类型的，reactive是包装数组或者对象的


//新增数据，设置新的空的绑值对象  打开弹窗
const handleAdd = () => {
  form = reactive({})
  dialogFormVisible.value = true//打开弹窗
}

//保存数据，将数据插入到tableData里面，并刷新页面数据，让弹窗关闭
const save = () => {
  if (globalIndex.value >= 0) {//表示编辑
    tableData.value[globalIndex.value] = form
    console.log(form)
    globalIndex.value = -1 //还原回去
  } else {//新增
    tableData.value.push(form)
  }
  dialogFormVisible.value = false
}

//编辑数据，先赋值数据到表单，再打开弹窗
const handleEdit = (row, index) => {
  const newObj = Object.assign({}, row)
  form = reactive(newObj)
  globalIndex.value = index //把当前编辑的行号赋值给 全局保存的编辑的行号

  dialogFormVisible.value = true
}

//删除数据，从index的位置删除一行
const remove = (index) => {
  console.log(1111);
  tableData.value.splice(index, 1)
}
const search = () => {
  tableData.value=tableData.value.filter(v => v.name.includes(name.value))
}
</script>

<style>

</style>