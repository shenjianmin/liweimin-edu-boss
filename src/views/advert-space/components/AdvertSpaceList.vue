<template>
  <el-card>
    <el-button @click="add">添加广告位</el-button>
    <el-table :data="list" style="width: 100%" v-loading="loading">
      <el-table-column label="spaceKey" prop="spaceKey" width="100">
      </el-table-column>
      <el-table-column prop="name" label="广告位名称"></el-table-column>
      <el-table-column prop="createdTime" label="创建时间">
        <template slot-scope="scope">
          {{ scope.row.createdTime | dateFormat }}
        </template>
      </el-table-column>
      <el-table-column prop='updateTime' label="更新时间">
        <template slot-scope="scope">
          {{ scope.row.updateTime | dateFormat }}
        </template>
      </el-table-column>
      <el-table-column prop="address" label="操作">
        <template slot-scope="scope">
          <el-button type="text" @click="edit(scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog :title="dialogTitle" :visible.sync="dialogVisible" width="30%">
      <el-form
        ref="advertform"
        :model="advertform"
        label-width="100px"
        :rules="rules"
      >
        <el-form-item label="广告位名称" prop="name">
          <el-input v-model="advertform.name"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="handleSave">确 定</el-button>
      </span>
    </el-dialog>
  </el-card>
</template>

<script lang="ts">
import Vue from 'vue'
import { Form } from 'element-ui'
import {
  getAllSpaces,
  saveOrderUpdateAdvertSpace
} from '@/services/advert-space'
export default Vue.extend({
  name: 'advertSpaceList',
  data () {
    return {
      list: [],
      loading: false,
      dialogTitle: '添加分类',
      dialogVisible: false,
      advertform: {},
      rules: {
        name: [{ required: true, message: '请输入名称', trigger: 'change' }]
      }
    }
  },
  mounted () {
    this.getAdvertSpacelist()
  },
  methods: {
    async getAdvertSpacelist () {
      this.loading = true
      const { data } = await getAllSpaces()
      console.log(data)
      this.list = data.content
      this.loading = false
    },
    edit (item: any) {
      this.dialogTitle = '编辑'
      this.dialogVisible = true
      this.advertform = JSON.parse(JSON.stringify(item))
    },
    add () {
      this.dialogTitle = '添加分类'
      this.dialogVisible = true
    },
    delete (item: any) {
      console.log(item)
    },
    handleSave () {
      (this.$refs.advertform as Form).validate(async (valid) => {
        if (valid) {
          const { data } = await saveOrderUpdateAdvertSpace(this.advertform)
          if (data.success === true) {
            this.$message({
              type: 'success',
              message: '操作成功!'
            })
            this.advertform = {}
            this.getAdvertSpacelist()
            this.dialogVisible = false
          }
        }
      })
    }
  }
})
</script>

<style lang="scss" scoped></style>
