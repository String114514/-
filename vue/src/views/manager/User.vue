<template>
  <div class="card" style="margin-bottom: 5px">
    <el-input style="width: 300px;margin-right: 10px;" v-model="data.name" placeholder="请输入名称查询"
              :prefix-icon="Search"/>
    <el-button @click="load" type="primary">查询</el-button>
    <el-button @click="reset" type="info" style="margin: 0 10px">重置</el-button>
  </div>
  <div class="card" style="margin-bottom: 5px;">
    <div style="margin-bottom: 5px">
      <!-- 注意：你原代码里新增按钮的click事件绑成了load，建议后续改成新增的方法，这里先保留你的原代码 -->
      <el-button @click="handleAdd" type="primary">新增</el-button>
    </div>
    <div>
      <el-table :data="data.tableData" stripe style="width: 100%">
        <el-table-column prop="username" label="账号"/>
        <el-table-column prop="name" label="姓名"/>
        <el-table-column prop="role" label="角色"/>
        <el-table-column prop="account" label="账户余额"/>
        <el-table-column label="操作" width="180">
          <template #default="scope">
            <el-button type="primary">编辑</el-button>
            <el-button type="danger" @click="del(scope.row.id)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
  <div class="card" style="display: flex; justify-content: space-between; align-items: center;">
    <!-- 总人数展示：放在分页左侧/右侧都可，这里放左侧，分页在右侧，视觉更协调 -->
    <!-- 分页组件：去掉原layout的多余配置，保留核心功能 -->
    <el-pagination
        v-model:current-page="data.pageNum"
        v-model:page-size="data.pageSize"
        @current-change="load"
        background
        layout="prev, pager, next"
        :total="data.total"
    />
    <div style="font-size: 14px; color: #666;">
      共<span style="font-weight: 600; color: #1890ff;">{{ data.total }}</span>条
    </div>

  </div>

  <el-dialog title="用户信息" v-model="data.formVisible" width="30%">
    <el-form :model="data.form" label-width="80px" style="padding-right: 30px">
      <el-form-item label="账号">
        <el-input v-model="data.form.username" placeholder="请输入账号" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="姓名">
        <el-input v-model="data.form.name" placeholder="请输入姓名" autocomplete="off"></el-input>
      </el-form-item>

    </el-form>
    <template #footer>
    <span class="dialog-footer">
      <el-button @click="data.formVisible = false">取 消</el-button>
      <el-button type="primary" @click="save">确 定</el-button>
    </span>
    </template>
  </el-dialog>

</template>

<script setup>
import {reactive} from "vue";
import {Search} from "@element-plus/icons-vue";
import request from "@/utils/request";
import {ElMessage, ElMessageBox} from "element-plus";

const data = reactive({
  name: null,
  tableData: [],
  total: 0, // 总人数/总数据量，后端已返回，直接使用
  pageNum: 1,
  pageSize: 5,
  formVisible: false,
  form: {
  }
})
// 分页查询数据的函数
const load = () => {
  request.get('/user/selectPage', {
    params: {
      pageNum: data.pageNum,
      pageSize: data.pageSize,
      name: data.name // 补充：加上名称查询参数，原代码漏了，否则查询功能无效
    }
  }).then(res => {
    if (res.code === '200') {
      data.tableData = res.data?.list
      data.total = res.data?.total // 后端返回的总条数，直接作为总人数
    } else {
      ElMessage.error(res.msg)
    }
  })
}
load()
// 重置函数：补充重置逻辑，原代码为空
const reset = () => {
  data.name = null; // 清空查询输入框
  data.pageNum = 1; // 重置页码为第一页
  load(); // 重新加载数据
}

const del = (id) => {
  ElMessageBox.confirm('您确定删除吗', '删除确认', {type: 'warning'}).then(res => {
    request.delete('/user/delete/' + id).then(res => {
      if (res.code === '200') {
        ElMessage.success('操作成功')
        // 删除之后刷新
        load()
      } else {
        ElMessage.error(res.msg)
      }
    })
  }).catch(err => {})
}

const handleAdd = () => {
  data.form = {}
  data.formVisible = true
}

const save = () => {
  request.post('/user/add',data.form).then(res => {
    if (res.code === '200') {
      ElMessage.success('操作成功')
      data.formVisible = false
      load()
    } else {
      ElMessage.error(res.msg)
    }
  })
}
</script>
