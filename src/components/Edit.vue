<script setup>
// TODO: 编辑
import { ref ,inject} from 'vue'
import {ElMessage} from 'element-plus'
import axios from 'axios'
// 弹框开关
const dialogVisible = ref(false)
const form=ref({

})
//声明一个方法接收要渲染的数据
const open=(row)=>{
  // console.log(row)
  dialogVisible.value=true
  // console.log(row)
  form.value={...row}
}


// const emit=defineEmits(['get-list'])
const getUserList=inject('getUserList')
const confirmEdit=async()=>{
  await axios.patch(`/edit/${form.value.id}`, {
  name: form.value.name,
  place: form.value.place,
})
  ElMessage.success('编辑成功')
  dialogVisible.value=false
  // emit('get-list')
  getUserList()
}


defineExpose({
  open
})
</script>

<template>
  <el-dialog v-model="dialogVisible" title="编辑" width="400px">
    <el-form label-width="50px">
      <el-form-item label="姓名">
        <el-input placeholder="请输入姓名" v-model="form.name"/>
      </el-form-item>
      <el-form-item label="籍贯">
        <el-input placeholder="请输入籍贯" v-model="form.place"/>
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="confirmEdit" >确认</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<style scoped>
.el-input {
  width: 290px;
}
</style>
