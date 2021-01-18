<template>
  <el-card>
    <el-form label-width="80px" :rules="rules" ref="advert" :model="advert" >
      <el-form-item label="广告名称" prop="name">
          <el-input v-model="advert.name"></el-input>
      </el-form-item>
      <el-form-item label="广告位置">
          <el-select v-model="advert.spaceId" placeholder="请选择广告位">
              <el-option :label="item.name" :value="item.id" v-for="item in spacelist" :key="item.id"></el-option>
          </el-select>
      </el-form-item>
      <el-form-item label="开始时间" prop="startTime">
          <el-date-picker v-model="advert.startTime" type="datetime" placeholder="选择日期时间"/>
      </el-form-item>
      <el-form-item label="到期时间" prop="endTime">
          <el-date-picker v-model="advert.endTime" type="datetime" placeholder="选择日期时间"/>
      </el-form-item>
      <el-form-item label="上线/下线">
          <el-switch
        v-model="advert.status"
        active-color="#13ce66"
        inactive-color="#ff4949"
        :active-value="1"
        :inactive-value="0"
      />
      </el-form-item>
      <el-form-item label="广告图片">
          <advert-image v-model="advert.img" :limit="1" />
          <el-tag>建议尺寸：230*300px，JPG格式，图片小于1M</el-tag>
      </el-form-item>
      <el-form-item label="广告链接" prop="link">
          <el-input v-model="advert.link"></el-input>
      </el-form-item>
      <el-form-item label="广告备注">
          <el-input type="textarea" v-model="advert.text"></el-input>
      </el-form-item>
      <el-form-item>
          <el-button type="primary" @click="handleSave">保存</el-button>
          <el-button  @click="$router.push('/advert')">返回</el-button>
      </el-form-item>
    </el-form>
  </el-card>
</template>

<script lang="ts">
import Vue from 'vue'
import { saveOrUpdateAd, getAdById } from '@/services/advert'
import AdvertImage from './AdvertImage.vue'
import { getAllSpaces } from '@/services/advert-space'
import moment from 'moment'
import { Form } from 'element-ui'
export default Vue.extend({
  name: 'CreateOrUpdateAdvert',
  props: {
    isEdit: {
      type: Boolean,
      default: false
    },
    advertId: {
      type: [String, Number]
    }
  },
  components: {
    AdvertImage
  },
  data () {
    return {
      advert: {},
      spacelist: [],
      rules: {
        name: [
          { required: true, message: '请输入名称', trigger: 'change' }
        ],
        startTime: [
          { required: true, message: '请选择开始时间', trigger: 'change' }
        ],
        endTime: [
          { required: true, message: '请选择到期时间', trigger: 'change' }
        ],
        link: [
          { required: true, message: '请输入广告链接', trigger: 'change' }
        ]
      }
    }
  },
  created () {
    if (this.isEdit) {
      this.loadCourse()
    }
    this.loadSpace()
  },
  methods: {
    async loadCourse () {
      const { data } = await getAdById(this.advertId)
      const advert = data.content
      advert.startTime = moment(advert.startTime).format('YYYY-MM-DD')
      advert.endTime = moment(advert.endTime).format('YYYY-MM-DD')
      this.advert = advert
    },
    async loadSpace () {
      const { data } = await getAllSpaces()
      this.spacelist = data.content
    },
    async handleSave () {
      (this.$refs.advert as Form).validate(async (valid) => {
        if (valid) {
          const { data } = await saveOrUpdateAd(this.advert)
          if (data.message === 'success') {
            this.$message.success('保存成功')
            this.$router.push('/advert')
          } else {
            this.$message.error('保存失败')
          }
        }
      })
    }
  }
})
</script>

<style lang="scss" scoped>
.el-step {
    cursor: pointer;
}
</style>
