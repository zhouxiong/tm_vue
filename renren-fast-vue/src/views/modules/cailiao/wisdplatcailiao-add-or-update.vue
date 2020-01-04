<template>
  <el-dialog
    :title="!dataForm.clId ? '新增' : !this.str?'修改':'预览'"
    :close-on-click-modal="false"
    :visible.sync="visible"
    width="70%"
  >
    <el-row :gutter="24">
      <el-col :span="14">
        <el-form
          :inline="true"
          :model="dataForm"
          :rules="dataRule"
          ref="dataForm"
          @keyup.enter.native="dataFormSubmit()"
          label-width="80px"
        >
          <el-form-item label="材料编号" prop="clNo">
            <el-input
              v-model="dataForm.clNo"
              placeholder="材料编号"
              @keyup.native="proving"
              :disabled="this.str?true:false"
            ></el-input>
          </el-form-item>
          <el-form-item label="材料名称" prop="clName">
            <el-input
              v-model="dataForm.clName"
              placeholder="材料名称"
              @keyup.native="proving"
              :disabled="this.str?true:false"
            ></el-input>
          </el-form-item>
          <el-form-item label="材料牌号" prop="clPaihao">
            <el-input
              v-model="dataForm.clPaihao"
              placeholder="材料牌号"
              @keyup.native="proving"
              :disabled="this.str?true:false"
            ></el-input>
          </el-form-item>
          <el-form-item label="温度" prop="clWendu"  >
            <el-input v-model="dataForm.clWendu" placeholder="温度" :disabled="this.str?true:false" @keyup.native="proving"></el-input>
          </el-form-item>
          <el-form-item label="厂家" prop="clChangjia">
            <el-input
              v-model="dataForm.clChangjia"
              placeholder="厂家"
              @keyup.native="proving"
              :disabled="this.str?true:false"
            ></el-input>
          </el-form-item>
          <el-form-item label="泊松比" prop="clBosongbi">
            <el-input
              v-model="dataForm.clBosongbi"
              placeholder="泊松比"
              :disabled="this.str?true:false"
               @keyup.native="proving"
            ></el-input>
          </el-form-item>
          <el-form-item label="材料密度(t/mm3)" prop="clMidu">
            <el-input
              v-model="dataForm.clMidu"
              placeholder="材料密度(t/mm3)"
              :disabled="this.str?true:false"
              @keyup.native="proving"
            ></el-input>
          </el-form-item>

          <el-form-item label="弹性模量(Mpa)" prop="clMoliang">
            <el-input
              v-model="dataForm.clMoliang"
              placeholder="弹性模量(Mpa)"
              :disabled="this.str?true:false"
              @keyup.native="proving"
            ></el-input>
          </el-form-item>
          <el-form-item label="SIGY(Mpa)" prop="clSigy">
            <el-input
              v-model="dataForm.clSigy"
              placeholder="SIGY(Mpa)"
              :disabled="this.str?true:false"
              @keyup.native="proving"
            ></el-input>
          </el-form-item>
          <el-form-item label="ETAN" prop="clEtan">
            <el-input v-model="dataForm.clEtan" placeholder="ETAN" :disabled="this.str?true:false" @keyup.native="proving"></el-input>
          </el-form-item>
          <el-form-item label="FAIL" prop="clFail">
            <el-input v-model="dataForm.clFail" placeholder="FAIL" :disabled="this.str?true:false" @keyup.native="proving"></el-input>
          </el-form-item>
          <el-form-item label="C" prop="clC">
            <el-input v-model="dataForm.clC" placeholder="C" :disabled="this.str?true:false" @keyup.native="proving"></el-input>
          </el-form-item>
          <el-form-item label="P" prop="clP">
            <el-input v-model="dataForm.clP" placeholder="P" :disabled="this.str?true:false" @keyup.native="proving"></el-input>
          </el-form-item>
          <el-form-item label="分类" prop="clFenleiId">
            <el-select
              v-model="dataForm.clFenleiId"
              placeholder="材料类型"
              ref="clFenlei"
              :disabled="this.str?true:false"
            >
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <!-- <el-form-item label="Excel文件地址" prop="clFileAddr">
      <el-input v-model="dataForm.clFileAddr" placeholder="Excel文件地址"></el-input>
    </el-form-item>
    <el-form-item label="创建人" prop="clCreatePerson">
      <el-input v-model="dataForm.clCreatePerson" placeholder="创建人"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="clCreateDate">
      <el-input v-model="dataForm.clCreateDate" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="修改人" prop="clUpdatePerson">
      <el-input v-model="dataForm.clUpdatePerson" placeholder="修改人"></el-input>
    </el-form-item>
    <el-form-item label="修改时间" prop="clUpdateDate">
      <el-input v-model="dataForm.clUpdateDate" placeholder="修改时间"></el-input>
          </el-form-item>-->
        </el-form>
      </el-col>
      <el-col :span="10">
        <!--
        <el-button  size="small" type="primary" @click="hanleClick" class="chooseButton"  style="margin-bottom:10px;">选取Excel文件</el-button>
        -->
          <el-upload
            class="upload-demo"
            drag
            :before-upload="beforeUpload"
            :on-exceed="handleExceed"
            :limit="1"
            :http-request="uploadFile"
            multiple
            ref="upload"
            action
            :disabled="this.str?true:false"
          >
            <i class="el-icon-upload"></i>
            <span class="el-upload__text">
                将文件拖到此处，或
                <em>点击上传</em>
            </span>

            <div class="el-upload__tip" slot="tip">只能上传Excel文件，且不超过500kb</div>
          </el-upload>

          <!-- <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div> -->
          <input
            type="file"
            name="filename"
            id="openfile"
            style="display:none;"
            @change="excelFile()"
          />
        <!-- <tmchart ref="addOrUpaterChart" :sendData="this.curveIdList" :width="600" :height="400"></tmchart> -->
        <br />
        <!-- <div id="myChart" class="chart-box" :option="option" :width="600" :height="400"></div> -->

        <!-- <keep-alive exclude="home"> -->
        <div id="myChart" :style="{width: '400px', height: '400px'}" :option="option"></div>
        <!-- </keep-alive> -->
      </el-col>
    </el-row>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
/* eslint-disable */
import tmchart from "@/views/demo/info/tmchart";
// import echarts from 'echarts'
export default {
  components: { tmchart },
  data() {
    return {
      changeRead: false,
      str: "",
      files: [],
      fileid: "",
      chartLine: null,
      curveIdList: [],
      value: "",
      options: [
        {
          value: "金属",
          label: "金属"
        },
        {
          value: "非金属",
          label: "非金属"
        }
      ],
      visible: false,
      dataForm: {
        clId: 0,
        clNo: "",
        clName: "",
        clPaihao: "",
        clWendu: "",
        clChangjia: "",
        clMidu: "",
        clBosongbi: "",
        clMoliang: "",
        clSigy: "",
        clEtan: "",
        clFail: "",
        clC: "",
        clP: "",
        clFenleiId: "",
        clFileAddr: "",
        clCreatePerson: "",
        clCreateDate: "",
        clUpdatePerson: "",
        clUpdateDate: ""
      },
      dataRule: {
        clNo: [
          { required: true, message: "材料编号不能为空", trigger: "blur" }
        ],
        clName: [
          { required: true, message: "材料名称不能为空", trigger: "blur" }
        ],
        clPaihao: [
          { required: true, message: "材料牌号不能为空", trigger: "blur" }
        ],
        clWendu: [{ required: true, message: "温度不能为空", trigger: "blur" }],
        clChangjia: [
          { required: true, message: "厂家不能为空", trigger: "blur" }
        ],
        clBosongbi: [
          { required: true, message: "泊松比不能为空", trigger: "blur" }
        ],
        clMoliang: [
          { required: true, message: "弹性模量(Mpa)不能为空", trigger: "blur" }
        ],
        clFenleiId: [
          { required: true, message: "分类不能为空", trigger: "blur" }
        ]
        // clFileAddr: [
        //   { required: true, message: "Excel文件地址不能为空", trigger: "blur" }
        // ],
        // clCreatePerson: [
        //   { required: true, message: "创建人不能为空", trigger: "blur" }
        // ],
        // clCreateDate: [
        //   { required: true, message: "创建时间不能为空", trigger: "blur" }
        // ],
        // clUpdatePerson: [
        //   { required: true, message: "修改人不能为空", trigger: "blur" }
        // ],
        // clUpdateDate: [
        //   { required: true, message: "修改时间不能为空", trigger: "blur" }
        // ]
      },
      option: {
        title: {
          text: "LCSS"
        },
        legend: {
          data: []
        },
        tooltip: {
          trigger: "item",
          axisPointer: {
            show: true,
            type: "cross",
            lineStyle: {
              type: "dashed",
              width: 1
            }
          }
        },
        toolbox: {
          show: true,
          feature: {
            // mark: { show: true },
            // dataZoom: { show: true },
            dataView: { show: true, readOnly: false },
            // magicType: { show: true, type: ["line", "bar"] },
            // restore: { show: true },
            // saveAsImage: { show: true }
          }
        },
        //   option: Line_options,
        xAxis: {
          // type: "category",
          boundaryGap: false,
          name: "X-应变",
          // data: ['A','B','C','D','E','F','G']
          // data: ""
          type: "value"
        },
        yAxis: {
          type: "value",
          name: "Y-应力"
        },
        series: []
      }
    };
  },
  mounted() {
    // this.drawLine();
  },

  methods: {
    init(id, str) {
      this.dataForm.clId = id || 0;
      this.str = str;
      // if (this.str) {
      //   this.changeRead = true;

      // }
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        // this.drawLine();
        if (this.dataForm.clId) {
          this.$http({
            url: this.$http.adornUrl(
              `/cailiao/wisdplatcailiao/info/${this.dataForm.clId}`
            ),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.clNo = data.wisdplatCailiao.clNo;
              this.dataForm.clName = data.wisdplatCailiao.clName;
              this.dataForm.clPaihao = data.wisdplatCailiao.clPaihao;
              this.dataForm.clWendu = data.wisdplatCailiao.clWendu;
              this.dataForm.clChangjia = data.wisdplatCailiao.clChangjia;
              this.dataForm.clMidu = data.wisdplatCailiao.clMidu;
              this.dataForm.clBosongbi = data.wisdplatCailiao.clBosongbi;
              this.dataForm.clMoliang = data.wisdplatCailiao.clMoliang;
              this.dataForm.clSigy = data.wisdplatCailiao.clSigy;
              this.dataForm.clEtan = data.wisdplatCailiao.clEtan;
              this.dataForm.clFail = data.wisdplatCailiao.clFail;
              this.dataForm.clC = data.wisdplatCailiao.clC;
              this.dataForm.clP = data.wisdplatCailiao.clP;
              this.dataForm.clFenleiId = data.wisdplatCailiao.clFenleiId;
              this.dataForm.clFileAddr = data.wisdplatCailiao.clFileAddr;
              this.dataForm.clCreatePerson =
                data.wisdplatCailiao.clCreatePerson;
              this.dataForm.clCreateDate = data.wisdplatCailiao.clCreateDate;
              this.dataForm.clUpdatePerson =
                data.wisdplatCailiao.clUpdatePerson;
              this.dataForm.clUpdateDate = data.wisdplatCailiao.clUpdateDate;
              this.curveIdList = data.wisdplatCailiao.curveIdList;
              this.drawLine();
            }
          });
        } else {
          // 基于准备好的dom，初始化echarts实例 防止渲染的数据没有清除
          const myChart = this.$echarts.init(
            document.getElementById("myChart")
          );
          //清空数据，否则视图曲线会叠加
          this.option.series = [];
          myChart.setOption(this.option, true);
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      this.$refs["dataForm"].validate(valid => {
        if (valid) {
          const fileObj = this.file;
          var fileData = new FormData();
          if ("" != fileObj && null != fileObj && undefined != fileObj) {
            fileData.append("file", fileObj);
          }
          fileData.append("clId", this.dataForm.clId);
          fileData.append("clNo", this.dataForm.clNo);
          fileData.append("clName", this.dataForm.clName);
          fileData.append("clPaihao", this.dataForm.clPaihao);
          fileData.append("clWendu", this.dataForm.clWendu);
          fileData.append("clChangjia", this.dataForm.clChangjia);
          fileData.append("clMidu", this.dataForm.clMidu);
          fileData.append("clBosongbi", this.dataForm.clBosongbi);
          fileData.append("clMoliang", this.dataForm.clMoliang);
          fileData.append("clSigy", this.dataForm.clSigy);
          fileData.append("clEtan", this.dataForm.clEtan);
          fileData.append("clFail", this.dataForm.clFail);
          fileData.append("clC", this.dataForm.clC);
          fileData.append("clP", this.dataForm.clP);
          fileData.append("clFenleiId", this.dataForm.clFenleiId);
          fileData.append("clFileAddr", this.dataForm.clFileAddr);
          let headers = {
            "Content-Type": "multipart/form-data"
          };
          this.uploading = true;

          this.$http({
            url: this.$http.adornUrl(
              `/cailiao/wisdplatcailiao/${
                !this.dataForm.clId ? "save" : "update"
              }`
            ),
            method: "post",
            headers: headers,
            data: fileData
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.uploadDialog = false;
              this.$refs.upload.clearFiles();
              this.file='';
              this.$message({
                message: "操作成功",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.visible = false;
                  this.$emit("refreshDataList");
                }
              });
            } else {
              this.$message.error(data.msg);
            }
          });
        }
      });
    },
    drawLine() {
      // 基于准备好的dom，初始化echarts实例
      const myChart = this.$echarts.init(document.getElementById("myChart"));
      //清空数据，否则视图曲线会叠加
      if (this.option.series.length > 0) {
        this.option.series = [];
      }
      // 绘制图表
      const n = {},
        r = [],
        x = []; //n为hash表，r为临时数组
      for (
        var i = 0;
        i < this.curveIdList.length;
        i++ //遍历当前数组)
      ) {
        if (
          this.curveIdList[i].wcXishu != null &&
          this.curveIdList[i].wcXishu != ""
        ) {
          if (
            this.curveIdList[i].wcNo != null &&
            this.curveIdList[i].wcNo != ""
          ) {
            if (!n[this.curveIdList[i].wcNo]){
              n[this.curveIdList[i].wcNo] = true;
              r.push(this.curveIdList[i].wcNo);
              x.push(this.curveIdList[i].wcX);
          
            }
          // if (this.curveIdList[i].wcNo === r[0]) {
                // x.push(this.curveIdList[i].wcX);
           //}
          }
        }
        //if (!this.curveIdList[i].wcXishu) {
        //if (!n[this.curveIdList[i].wcNo]) {
        //如果hash表中没有当前项
        //const temp = {};
        //n[this.curveIdList[i].wcNo] = true; //存入hash表
        //r.push(this.curveIdList[i].wcNo); //把当前数组的当前项push到临时数组里面
        //}
        //if (this.curveIdList[i].wcNo === r[0]) {
        //x.push(this.curveIdList[i].wcX);
        //}
        //}
      }
      //根据 曲线ID号进行 Y坐标 分组;
      for (var i = 0; i < r.length; i++) {
        let tempList = []; //临时变量
        let temp = {};
        for (
          var k = 0;
          k < this.curveIdList.length;
          k++ //
        ) {
          if (r[i] === this.curveIdList[k].wcNo) {
            this.$set(temp, "name", this.curveIdList[k].wcNo);
            this.$set(temp, "type", "line");
            //  this.option.series.push(temp)
            //tempList.push(this.curveIdList[k].wcY)
            tempList.push([this.curveIdList[k].wcX, this.curveIdList[k].wcY]);
          }
        }
        this.$set(temp, "data", tempList);
        // this.option.series='';
        this.option.series.push(temp);
      }
      this.option.legend.data = r;
      //this.option.xAxis.data = x;
      myChart.setOption(this.option, true);
      this.option.series = [];
    },

    hanleClick(event) {
      //执行上传功能
      const fileObj = this.file;
      var fileData = new FormData();
      fileData.append("file", fileObj);
      let headers = {
        "Content-Type": "multipart/form-data"
      };
      this.uploading = true;
      this.$http({
        method: "post",
        url: this.$http.adornUrl(`/cailiao/wisdplatcurve/uploadExcel`),
        headers: headers,
        data: fileData
      }).then(res => {
        if (res.data.code === 200) {
          this.$message.success(res.data.msg);
          this.uploadDialog = false;
        } else {
          this.$message.error(res.data.msg);
        }
      });
      setTimeout(function() {
        this.uploading = false;
      }, 1500);
    },
    beforeUpload(file) {
      if (file.type == "" || file.type == null || file.type == undefined) {
        const FileExt = file.name.replace(/.+\./, "").toLowerCase();
        if (
          FileExt == "xls" ||
          FileExt == "xlsx" ||
          FileExt == "XLS" ||
          FileExt == "XLSX"
        ) {
          return true;
        } else {
          this.$message.error("上传文件必须是Excel格式!");
          return false;
        }
      } else {
        const isText = file.type === "application/vnd.ms-excel";
        const isTextComputer =
          file.type ===
          "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet";
        if (!isText && !isTextComputer) {
          this.$message.error("上传文件必须是Excel格式!");
        }
        return isText || isTextComputer;
      }
    },
    //上传文件个数超过定义的数量
    handleExceed(files, fileList) {
      this.$message.warning(`当前限制选择 1 个文件，请删除后继续上传`);
    },
    uploadFile(item) {
      this.file = item.file;
    },
    excelFile() {
      let msg = new FormData();
      msg.append("file", document.getElementById("openfile").files[0]);
      let config = {
        headers: { "Content-Type": "multipart/form-data" }
      };
      //var instance = axios.create({
      //headers: {'content-type': 'application/x-www-form-urlencoded'}
      //});
      this.$http
        .post("/cailiao/wisdplatcurve/uploadExcel", msg, config)
        .then(response => {
          console.log(response.data);
        });
    },
    //正则表达式判断
    proving() {
      this.dataForm.clNo = this.dataForm.clNo.replace(/[^\.\d]/g, ""); //输入数字
      this.dataForm.clName = this.dataForm.clName.replace(/[\W]/g, ""); //数字, 字符, _ .
      this.dataForm.clPaihao = this.dataForm.clPaihao.replace(/[\W]/g, "");
      this.dataForm.clChangjia = this.dataForm.clChangjia.replace(/[\W]/g, "");
      this.dataForm.clWendu = this.dataForm.clWendu.replace(/[^\.\d]/g, "");
      this.dataForm.clMidu = this.dataForm.clMidu.replace(/[^\.\d]/g, "");
      this.dataForm.clC = this.dataForm.clC.replace(/[^\.\d]/g, "");
      this.dataForm.clP = this.dataForm.clP.replace(/[^\.\d]/g, "");
      this.dataForm.clBosongbi = this.dataForm.clBosongbi.replace(/[^\.\d]/g, "");
      this.dataForm.clMoliang = this.dataForm.clMoliang.replace(/[^\.\d]/g, "");
      this.dataForm.clEtan = this.dataForm.clEtan.replace(/[^\.\d]/g, "");
      this.dataForm.clFail = this.dataForm.clFail.replace(/[^\.\d]/g, "");
      this.dataForm.clSigy = this.dataForm.clSigy.replace(/[^\.\d]/g, "");
    
    }
  }
};
</script>
<style >
.el-upload-dragger  {
  background-color: #fff;
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  width: 150%;
  height: 60px;
  text-align: center;
  position: relative;
  overflow: hidden;
}
.el-upload-dragger .el-icon-upload {
  font-size: 67px;
  color: #c0c4cc;
  margin: 0px 0 0px;
  line-height: 50px;
  /* float:left; */
}

.el-dialog__body {
    padding: 30px 20px;
    color:
    #606266;
    line-height: 24px;
    font-size: 14px;
}
</style>
