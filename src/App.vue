<script setup>
import Edit from './components/Edit.vue'

import axios from 'axios'

import { ref,onMounted,provide} from 'vue'

import {ElMessageBox,ElMessage} from 'element-plus'

// TODO: 列表渲染
//二次封装
const list =ref([])
const getList=async ()=>{
  const res=await axios.get('/list')
  // console.log(res.data)
  list.value=res.data
}

onMounted(()=>{
  getList()
})


// TODO: 删除功能
function delUser(id){
  // console.log(id)
  ElMessageBox.confirm('您确定要删除吗？','温馨提示').then(async()=>{
    await axios.delete(`/del/${id}`)
    ElMessage.success('删除')
    getList()
  }).catch(()=>{

  })
}




// TODO: 编辑功能

const editRef=ref(null)
//点击编辑按钮
const editUser=row=>{
  // console.log(row)
  editRef.value.open(row)
}


provide('getUserList',getList)

</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="scope">
          <el-button type="primary" link @click="editUser(scope.row)">编辑</el-button>
          <el-button type="danger" link @click="delUser(scope.row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="editRef" @get-list="getList"/>
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
