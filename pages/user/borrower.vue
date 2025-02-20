<template>
  <div class="personal-main">
    <div class="personal-pay" v-loading="loading">
      <h3><i>借款人信息认证</i></h3>

      <el-steps :active="active" style="margin: 40px" finish-status="success">
        <el-step title="填写借款人信息"></el-step>
        <el-step title="提交平台审核"></el-step>
        <el-step title="等待认证结果"></el-step>
      </el-steps>

      <div v-if="active === 0" class="user-borrower">
        <h6>个人基本信息</h6>
        <el-form label-width="120px">
          <el-form-item label="年龄">
            <el-col :span="5">
              <el-input v-model="borrower.age" />
            </el-col>
          </el-form-item>
          <el-form-item label="性别">
            <el-select v-model="borrower.sex">
              <el-option :value="1" :label="'男'"></el-option>
              <el-option :value="0" :label="'女'"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="是否结婚">
            <el-select v-model="borrower.marry">
              <el-option :value="true" :label="'是'"></el-option>
              <el-option :value="false" :label="'否'"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="学历">
            <el-select v-model="borrower.education">
              <el-option
                v-for="item in educationList"
                :key="item.value"
                :label="item.name"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="行业">
            <el-select v-model="borrower.industry">
              <el-option
                v-for="item in industryList"
                :key="item.value"
                :label="item.name"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="月收入">
            <el-select v-model="borrower.income">
              <el-option
                v-for="item in incomeList"
                :key="item.value"
                :label="item.name"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="还款来源">
            <el-select v-model="borrower.returnSource">
              <el-option
                v-for="item in returnSourceList"
                :key="item.value"
                :label="item.name"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
        </el-form>

        <h6>联系人信息</h6>
        <el-form label-width="120px">
          <el-form-item label="联系人姓名">
            <el-col :span="5">
              <el-input v-model="borrower.contactsName"></el-input>
            </el-col>
          </el-form-item>
          <el-form-item label="联系人手机">
            <el-col :span="5">
              <el-input v-model="borrower.contactsMobile"></el-input>
            </el-col>
          </el-form-item>
          <el-form-item label="联系人关系">
            <el-select v-model="borrower.contactsRelation">
              <el-option
                v-for="item in contactsRelationList"
                :key="item.value"
                :label="item.name"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
        </el-form>
        <h6>身份认证信息</h6>
        <el-form label-width="120px">
          <el-form-item label="身份证人像面">
            <!-- 在文件上传时，:data 中绑定的内容会被加入【post 请求体】提交给后台 -->
            <el-upload
              :action="uploadUrl"
              :data="{ module: 'idCard1' }"
              :on-remove="onUploadRemove"
              :on-success="onUploadSuccessIdCard1"
              :multiple="false"
              :limit="1"
              list-type="picture-card"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
          </el-form-item>
          <el-form-item label="身份证国徽面">
            <el-upload
              :action="uploadUrl"
              :data="{ module: 'idCard2' }"
              :on-remove="onUploadRemove"
              :on-success="onUploadSuccessIdCard2"
              :multiple="false"
              :limit="1"
              list-type="picture-card"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
          </el-form-item>
        </el-form>

        <h6>其他信息</h6>
        <el-form label-width="120px">
          <el-form-item label="房产信息">
            <el-upload
              :action="uploadUrl"
              :data="{ module: 'house' }"
              :on-remove="onUploadRemove"
              :on-success="onUploadSuccessHouse"
              :multiple="false"
              :limit="1"
              list-type="picture-card"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
          </el-form-item>
          <el-form-item label="车辆信息">
            <el-upload
              :action="uploadUrl"
              :data="{ module: 'car' }"
              :on-remove="onUploadRemove"
              :on-success="onUploadSuccessCar"
              :multiple="false"
              :limit="1"
              list-type="picture-card"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
          </el-form-item>
        </el-form>

        <el-form label-width="120px">
          <el-form-item>
            <el-button
              type="primary"
              :disabled="submitBtnDisabled"
              @click="save()"
              >提交</el-button
            >
          </el-form-item>
        </el-form>
      </div>

      <div v-if="active === 1">
        <div style="margin-top: 40px">
          <el-alert
            type="warning"
            title="您的认证申请已成功提交，请耐心等待"
            show-icon
            :closable="false"
          >
            我们将在 2 小时内完成审核，审核时间为周一至周五、8:00 至 18:00。
          </el-alert>
        </div>
      </div>

      <div v-if="active === 2">
        <div style="margin-top: 40px">
          <el-alert
            v-if="borrowerStatus === 2"
            type="success"
            title="您的认证审批已经通过"
            show-icon
            :closable="false"
          ></el-alert>
          <el-alert
            v-if="borrowerStatus === -1"
            type="error"
            title="您的认证审批未通过"
            show-icon
            :closable="false"
          ></el-alert>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    let BASE_API = process.env.BASE_API

    return {
      active: null,
      // 初次进入页面时，显示加载中
      loading: null,
      // 借款人认证状态
      borrowerStatus: null,
      // 提交按钮是否禁用，防止表单重复提交
      submitBtnDisabled: false,
      //借款人信息
      borrower: {
        borrowerAttachList: [],
      },
      educationList: [], //学历列表
      industryList: [], //行业列表
      incomeList: [], //月收入列表
      returnSourceList: [], //还款来源列表
      contactsRelationList: [], //联系人关系
      uploadUrl: BASE_API + '/api/oss/upload', //文件上传地址
    }
  },

  mounted() {
    this.loading = true
    // 该方法需要放到 mounted 中，否则发送的请求不会被拦截和设置 token
    this.determineStatus()
  },

  methods: {
    /**
     * 确定借款人认证状态，根据状态显示不同的页面
     */
    determineStatus() {
      this.$axios
        .$get('/api/core/borrower/auth/getBorrowerStatus')
        .then((res) => {
          this.loading = false
          this.borrowerStatus = res.data.status
          if (this.borrowerStatus === 0) {
            this.active = 0
            this.initSelectionData()
          } else if (this.borrowerStatus === 1) {
            this.active = 1
          } else if (this.borrowerStatus === 2 || this.borrowerStatus === -1) {
            this.active = 2
          }
        })
    },
    save() {
      this.submitBtnDisabled = true
      this.$axios
        .$post('/api/core/borrower/auth/save', this.borrower)
        .then(() => {
          this.active = 1
        })
    },
    onUploadRemove(file, fileList) {
      // console.log('fileList', fileList, file)
      // 删除 OSS 服务器上的内容
      this.$axios
        .$delete('/api/oss/remove?url=' + file.response.data.url)
        .then(() => {
          this.borrower.borrowerAttachList =
            this.borrower.borrowerAttachList.filter(function (item) {
              return item.imageUrl != file.response.data.url
            })
        })
    },
    onUploadSuccessIdCard1(response, file) {
      this.onUploadSuccess(response, file, 'idCard1')
    },
    onUploadSuccessIdCard2(response, file) {
      this.onUploadSuccess(response, file, 'idCard2')
    },
    onUploadSuccessHouse(response, file) {
      this.onUploadSuccess(response, file, 'house')
    },
    onUploadSuccessCar(response, file) {
      this.onUploadSuccess(response, file, 'car')
    },
    onUploadSuccess(response, file, moduleName) {
      if (response.code === 0) {
        this.borrower.borrowerAttachList.push({
          imageName: file.name,
          imageUrl: response.data.url,
          imageType: moduleName,
        })
      } else {
        this.$message.error(response.message)
      }
    },
    initSelectionData() {
      // 学历列表
      this.$axios
        .$get('/api/core/dict/findByDictCode/education')
        .then((res) => {
          this.educationList = res.data.list
        })

      // 行业列表
      this.$axios.$get('/api/core/dict/findByDictCode/industry').then((res) => {
        this.industryList = res.data.list
      })

      // 月收入列表
      this.$axios.$get('/api/core/dict/findByDictCode/income').then((res) => {
        this.incomeList = res.data.list
      })

      // 还款来源列表
      this.$axios
        .$get('/api/core/dict/findByDictCode/returnSource')
        .then((res) => {
          this.returnSourceList = res.data.list
        })

      // 联系人关系列表
      this.$axios.$get('/api/core/dict/findByDictCode/relation').then((res) => {
        this.contactsRelationList = res.data.list
      })
    },
  },
}
</script>
