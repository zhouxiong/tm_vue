<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="请求参数" prop="requestArgu">
      <el-input v-model="dataForm.requestArgu" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="请求日期" prop="requestDate">
      <el-date-picker
        v-model="dataForm.requestDate"
        type="date"
        placeholder="选择日期" value-format="yyyy-MM-dd"  format="yyyy-MM-dd">
      </el-date-picker>
    </el-form-item>
    <el-form-item label="响应参数" prop="responseArgu">
      <el-input v-model="dataForm.responseArgu" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="耗时" prop="useHours">
      <el-input v-model="dataForm.useHours" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="创建人" prop="createPerson">
      <el-input v-model="dataForm.createPerson" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="来源" prop="laiyuan">
      <el-input v-model="dataForm.laiyuan" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="接口类型" prop="ilType">
      <el-input v-model="dataForm.ilType" placeholder=""></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          ilId: 0,
          requestArgu: '',
          requestDate: '',
          responseArgu: '',
          useHours: '',
          createPerson: '',
          laiyuan: '',
          ilType: ''
        },
        dataRule: {
          requestArgu: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          requestDate: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          responseArgu: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          useHours: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          createPerson: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          laiyuan: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          ilType: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.ilId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.ilId) {
            this.$http({
              url: this.$http.adornUrl(`/sys/interlog/info/${this.dataForm.ilId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.requestArgu = data.interLog.requestArgu
                this.dataForm.requestDate = data.interLog.requestDate
                this.dataForm.responseArgu = data.interLog.responseArgu
                this.dataForm.useHours = data.interLog.useHours
                this.dataForm.createPerson = data.interLog.createPerson
                this.dataForm.laiyuan = data.interLog.laiyuan
                this.dataForm.ilType = data.interLog.ilType
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/sys/interlog/${!this.dataForm.ilId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'ilId': this.dataForm.ilId || undefined,
                'requestArgu': this.dataForm.requestArgu,
                'requestDate': this.dataForm.requestDate,
                'responseArgu': this.dataForm.responseArgu,
                'useHours': this.dataForm.useHours,
                'createPerson': this.dataForm.createPerson,
                'laiyuan': this.dataForm.laiyuan,
                'ilType': this.dataForm.ilType
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
