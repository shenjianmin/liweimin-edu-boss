<template>
  <el-card>
    <el-button @click="add">添加分类</el-button>
    <el-table
      :data="list"
      style="width: 100%"
      v-loading="loading"
    >
      <el-table-column
        type="index"
        label="编号"
        width="100">
      </el-table-column>
      <el-table-column
        prop="name"
        label="名称"
        >
      </el-table-column>
      <el-table-column
        prop="createdTime"
        label="创建时间"
      >
        <template slot-scope="scope">
          {{scope.row.createdTime|dateFormat}}
        </template>
      </el-table-column>
      <el-table-column
        prop="sort"
        label="排序"
      >
      </el-table-column>
      <el-table-column
        prop="address"
        label="操作"
      >
        <template slot-scope="scope">
          <el-button
            type="text"
            @click="edit(scope.row)"
          >编辑</el-button>
          <el-button
            type="text"
            @click="delete(scope.row)"
          >删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog
      :title="dialogTitle"
      :visible.sync="dialogVisible"
      width="30%"
    >
      <el-form ref="categoryform" :model="categoryform" label-width="80px" :rules="rules">
        <el-form-item label="名称" prop="name">
          <el-input v-model="categoryform.name" ></el-input>
        </el-form-item>
        <el-form-item label="排序" prop="sort">
          <el-input v-model="categoryform.sort" ></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button
          type="primary"
          @click="handleSave"
        >确 定</el-button>
      </span>
    </el-dialog>
  </el-card>
</template>

<script lang="ts">
import Vue from 'vue'
import { getResourceCategories, saveOrderUpdateCategories } from '@/services/resource-category'
import { Form } from 'element-ui'
export default Vue.extend({
  name: 'resourceCategoryList',
  data () {
    return {
      list: [],
      loading: false,
      dialogTitle: '添加分类',
      dialogVisible: false,
      categoryform: {},
      rules: {
        name: [
          { required: true, message: '请输入名称', trigger: 'change' }
        ],
        sort: [
          { required: true, message: '请输入排序', trigger: 'change' }
        ]
      }
    }
  },
  mounted () {
    this.getResourceCategorieslist()
  },
  methods: {
    async getResourceCategorieslist () {
      this.loading = true
      const { data } = await getResourceCategories()
      this.list = data.data
      this.loading = false
    },
    edit (item: any) {
      this.dialogTitle = '编辑'
      this.dialogVisible = true
      this.categoryform = JSON.parse(JSON.stringify(item))
    },
    add () {
      this.dialogTitle = '添加分类'
      this.dialogVisible = true
    },
    delete (item: any) {
      console.log(item)
    },
    handleSave () {
      (this.$refs.categoryform as Form).validate(async (valid) => {
        if (valid) {
          const { data } = await saveOrderUpdateCategories(this.categoryform)
          if (data.code === '000000') {
            this.$message({
              type: 'success',
              message: '操作成功!'
            })
            this.categoryform = {}
            this.getResourceCategorieslist()
            this.dialogVisible = false
          }
        }
      })
    }
  }
})
</script>

<style lang="scss" scoped></style>
