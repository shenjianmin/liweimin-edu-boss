<template>
  <div class="course-list">
    <el-card>
      <div slot="header">
        <el-button
          @click="$router.push({
            name: 'advert-create'
          })"
        >添加广告</el-button>
      </div>
      <el-table
        :data="advertlist"
        v-loading="loading"
        style="width: 100%; margin-bottom: 20px"
      >
        <el-table-column
          prop="id"
          label="ID"
          width="50">
        </el-table-column>
        <el-table-column
          prop="name"
          label="广告名称"
          >
        </el-table-column>
        <el-table-column
          prop="spaceId"
          label="广告位置" :formatter = "spaceIdFormat">
        </el-table-column>
        <el-table-column
          prop="img"
          label="广告图片">
          <template slot-scope="scope">
            <img :src="scope.row.img" alt="" style="width:200px">
          </template>
        </el-table-column>
         <el-table-column
          prop="createdTime"
          label="添加时间"
        >
          <template slot-scope="scope">
           开始时间： {{scope.row.startTime|dateFormat}} <br/>
           结束时间： {{scope.row.endTime|dateFormat}}
          </template>
        </el-table-column>
        <el-table-column
          prop="status"
          label="上线状态">
          <template slot-scope="scope">
            <el-switch
              v-model="scope.row.status"
              active-color="#13ce66"
              inactive-color="#ff4949"
              :active-value="1"
              :inactive-value="0"
              :disabled="scope.row.isStatusLoading"
              @change="onStateChange(scope.row)"
            />
          </template>
        </el-table-column>
        <el-table-column
          prop="price"
          label="操作"
          width="180"
          align="center"
        >
          <template slot-scope="scope">
            <el-button
              @click="$router.push({
                name: 'advert-edit',
                params: {
                  advertId: scope.row.id
                }
              })"
            >编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { getAdList, saveOrUpdateAd } from '@/services/advert'
import { getAllSpaces } from '@/services/advert-space'
export default Vue.extend({
  name: 'AdvertList',
  data () {
    return {
      advertlist: [],
      loading: true,
      spacelist: []
    }
  },
  created () {
    this.loadCourses()
    this.loadSpace()
  },
  methods: {
    async loadCourses () {
      this.loading = true
      const { data } = await getAdList()
      data.content.forEach((item: any) => {
        item.isStatusLoading = false
      })
      this.advertlist = data.content
      this.loading = false
    },
    async loadSpace () {
      const { data } = await getAllSpaces()
      this.spacelist = data.content
    },
    async onStateChange (advert: any) {
      advert.isStatusLoading = true
      console.log(advert)
      await saveOrUpdateAd(advert)
      this.$message.success(`${advert.status === 0 ? '下线' : '上线'}成功`)
      advert.isStatusLoading = false
    },
    spaceIdFormat (row: any) {
      const data = this.spacelist.find((item: any): boolean => {
        return item.id === row.spaceId
      })
      return data ? (data as any).name : ''
    }
  }
})
</script>

<style lang="scss" scoped>
.el-card {
  margin-bottom: 20px;
}
</style>
