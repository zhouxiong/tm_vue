<template>
    <el-dialog title="导出材料" :visible.sync="visible" :close-on-click-modal="false"  @keyup.enter.native="dataFormSubmit()">
        <el-form :model="dataForm"  :rules="dataRule" ref="dataForm" label-width="180px">
           <el-form-item label="导出文件类型" size="mini" prop="fileType">
              <el-radio-group v-model="dataForm.fileType" @change="changefileTypeHandler">
                 <el-radio   :label="1">Dyna</el-radio>
                 <el-radio  :label="2">ABAQUS</el-radio>
                 <el-radio   :label="3">Nastran</el-radio>
              </el-radio-group>
           </el-form-item>
          <el-form-item label="是否加密" size="mini" prop="jiami">
              <el-radio-group v-model="dataForm.jiami">
                  <el-radio :label="1">加密</el-radio>
                  <el-radio :label="2">不加密</el-radio>
              </el-radio-group>
          </el-form-item>

            <el-form-item label="文件路径" prop="fileurl">
               <!--
               <el-select v-model="value" placeholder="盘符" style="z-index:1000000;">
                  <el-option  v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
                  </el-option>
               </el-select>
               -->
              <el-input v-model="dataForm.fileurl" placeholder="文件名称" id="filename"></el-input>
            </el-form-item>



        </el-form>
        <span slot="footer" class="dialog-footer">
            <el-button @click="visible=false">取消</el-button>
            <el-button type="primary" @click="dataFormSubmit()">确定导出</el-button>
        </span>
    </el-dialog>

</template>

<script>
    import ElDialog from "element-ui/packages/dialog/src/component";
    import ElRadio from "element-ui/packages/radio/src/radio";
    import ElRadioGroup from "element-ui/packages/radio/src/radio-group";
    import ElInput from "element-ui/packages/input/src/input";
    import ElOption from "element-ui/packages/select/src/option";

    export default {
      components: {
        ElOption,
        ElInput,
        ElRadioGroup,
        ElRadio,
        ElDialog},
      name: "wisdplat-export.vue",
      data() {
        return {
          visible:false,
          dataForm: {
            fileType: 1,
            jiami: 1,
            fileurl:'Dyna'


          },
          dataRule: {
            fileurl: [
              {required: true,message: '文件名不能为空',trigger: 'blur'}
            ]
          },
          options: [{
            value: 'C',
            label: 'C盘'

          }, {
            value: 'D',
            label: 'D盘'
          }, {
            value: 'E',
            label: 'E盘'
          }, {
            value: 'F',
            label: 'F盘'
          }, {
            value: 'desktop',
            label: '桌面'
          }],
          value: 'C'

        }
      },
      methods: {
        init(id) {
          this.dataForm.clId=id;
          this.visible = true;

        },
        handleFile(file) {
          location.href = file.url;
        }
        ,
        changefileTypeHandler(value){
          let date = new Date();
          var year = date.getFullYear();
          var month = date.getMonth()+1;
          var day = date.getDate();
          var hour = date.getHours();
          var minute = date.getMinutes();
          var second = date.getSeconds();
          var milliseconds = date.getMilliseconds();
          if("1"==value){
            this.dataForm.fileurl='Dyna'+year+month+day+hour+minute+second+milliseconds;

          }
          if("2"==value){
            this.dataForm.fileurl='ABAQUS'+year+month+day+hour+minute+second+milliseconds;

          }
          if("3"==value){
            this.dataForm.fileurl='Nastran'+year+month+day+hour+minute+second+milliseconds;

          }
        },
        dataFormSubmit() {
          this.$refs['dataForm'].validate((valid) => {
            if(valid) {
              this.$http({
                url: this.$http.adornUrl(`/cailiao/wisdplatcailiao/exportcailiao/${this.dataForm.clId}`),
                method: 'post',
                data: this.$http.adornData({
                  'fileType': this.dataForm.fileType,
                  'jiami':this.dataForm.jiami,
                  'fileurl':this.dataForm.fileurl

                })
              }).then(({data}) => {
                if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                  this.visible = false

              }
              })
                this.$http({
                  url: this.$http.adornUrl(`/cailiao/wisdplatcailiao/download/${this.dataForm.clId}`),
                  method: 'post',
                  responseType: 'arraybuffer',
                  data: this.$http.adornData({
                    'fileType': this.dataForm.fileType,
                    'jiami':this.dataForm.jiami,
                    'fileurl':this.dataForm.fileurl

                  })
                }).then((res) => {
                  let url = window.URL.createObjectURL(new Blob([res.data]))
                  let a = document.createElement('a')
                  let type=this.dataForm.fileType
                  if('1'==type)
                {
                  a.setAttribute("download", this.dataForm.fileurl + ".k")
                }
                if('2'==type){
                  a.setAttribute("download", this.dataForm.fileurl + ".inp")
                }
                if('3'==type){
                  a.setAttribute("download", this.dataForm.fileurl + ".bdf")
                }
                  a.href = url
                  a.click();

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

<style scoped>

</style>
