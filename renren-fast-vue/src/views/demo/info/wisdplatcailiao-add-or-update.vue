<template>
  <el-dialog
    :title="!dataForm.clId ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible"
    width="70%"
  >
    <el-row :gutter="24">
      <el-col :span="12">
        <el-form
          :inline="true"
          :model="dataForm"
          :rules="dataRule"
          ref="dataForm"
          @keyup.enter.native="dataFormSubmit()"
          label-width="80px"
        >
          <el-form-item label="材料编号" prop="clNo">
            <el-input v-model="dataForm.clNo" placeholder="材料编号"></el-input>
          </el-form-item>
          <el-form-item label="材料名称" prop="clName">
            <el-input v-model="dataForm.clName" placeholder="材料名称"></el-input>
          </el-form-item>
          <el-form-item label="材料牌号" prop="clPaihao">
            <el-input v-model="dataForm.clPaihao" placeholder="材料牌号"></el-input>
          </el-form-item>
          <el-form-item label="温度" prop="clWendu">
            <el-input v-model="dataForm.clWendu" placeholder="温度"></el-input>
          </el-form-item>
          <el-form-item label="厂家" prop="clChangjia">
            <el-input v-model="dataForm.clChangjia" placeholder="厂家"></el-input>
          </el-form-item>
          <el-form-item label="泊松比" prop="clBosongbi">
            <el-input v-model="dataForm.clBosongbi" placeholder="泊松比"></el-input>
          </el-form-item>
          <el-form-item label="材料密度(t/mm3)" prop="clMidu">
            <el-input v-model="dataForm.clMidu" placeholder="材料密度(t/mm3)"></el-input>
          </el-form-item>

          <el-form-item label="弹性模量(Mpa)" prop="clMoliang">
            <el-input v-model="dataForm.clMoliang" placeholder="弹性模量(Mpa)"></el-input>
          </el-form-item>
          <el-form-item label="SIGY(Mpa)" prop="clSigy">
            <el-input v-model="dataForm.clSigy" placeholder="SIGY(Mpa)"></el-input>
          </el-form-item>
          <el-form-item label="ETAN" prop="clEtan">
            <el-input v-model="dataForm.clEtan" placeholder="ETAN"></el-input>
          </el-form-item>
          <el-form-item label="FAIL" prop="clFail">
            <el-input v-model="dataForm.clFail" placeholder="FAIL"></el-input>
          </el-form-item>
          <el-form-item label="C" prop="clC">
            <el-input v-model="dataForm.clC" placeholder="C"></el-input>
          </el-form-item>
          <el-form-item label="P" prop="clP">
            <el-input v-model="dataForm.clP" placeholder="P"></el-input>
          </el-form-item>
          <el-form-item label="分类" prop="clFenleiId">
            <el-select v-model="dataForm.clFenleiId" placeholder="材料类型">
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
      <el-col :span="12">
          <el-button  size="small" type="primary" @click="hanleClick" class="chooseButton"  style="margin-bottom:10px;">111选取Excel文件</el-button>
          <!-- <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div> -->
     
        <!-- <tmchart ref="addOrUpaterChart" :sendData="this.curveIdList" :width="600" :height="400"></tmchart> -->
        <br />
        <!-- <div id="myChart" class="chart-box" :option="option" :width="600" :height="400"></div> -->

        <div id="myChart" :style="{width: '600px', height: '400px'}" :option="option"></div>
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
        clMidu: [
          {
            required: true,
            message: "材料密度(t/mm3)不能为空",
            trigger: "blur"
          }
        ],
        clBosongbi: [
          { required: true, message: "泊松比不能为空", trigger: "blur" }
        ],
        clMoliang: [
          { required: true, message: "弹性模量(Mpa)不能为空", trigger: "blur" }
        ],
        clSigy: [
          { required: true, message: "SIGY(Mpa)不能为空", trigger: "blur" }
        ],
        clEtan: [{ required: true, message: "ETAN不能为空", trigger: "blur" }],
        clFail: [{ required: true, message: "FAIL不能为空", trigger: "blur" }],
        clC: [{ required: true, message: "C不能为空", trigger: "blur" }],
        clP: [{ required: true, message: "P不能为空", trigger: "blur" }],
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
          text: "LCSS曲线图"
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
            mark: { show: true },
            dataZoom: { show: true },
            dataView: { show: true, readOnly: false },
            // magicType: { show: true, type: ["line", "bar"] },
            restore: { show: true },
            saveAsImage: { show: true }
          }
        },
        //   option: Line_options,
        xAxis: {
          // type: "category",
          boundaryGap: false,
          // data: ['A','B','C','D','E','F','G']
          // data: ""
          type: "value"
        },
        yAxis: {
          type: "value"
        },
        series: []
      }
    };
  },
  mounted() {
    // this.drawLine();
  },

  methods: {
    init(id) {
      this.dataForm.clId = id || 0;
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
          this.$http({
            url: this.$http.adornUrl(
              `/cailiao/wisdplatcailiao/${
                !this.dataForm.clId ? "save" : "update"
              }`
            ),
            method: "post",
            data: this.$http.adornData({
              clId: this.dataForm.clId || undefined,
              clNo: this.dataForm.clNo,
              clName: this.dataForm.clName,
              clPaihao: this.dataForm.clPaihao,
              clWendu: this.dataForm.clWendu,
              clChangjia: this.dataForm.clChangjia,
              clMidu: this.dataForm.clMidu,
              clBosongbi: this.dataForm.clBosongbi,
              clMoliang: this.dataForm.clMoliang,
              clSigy: this.dataForm.clSigy,
              clEtan: this.dataForm.clEtan,
              clFail: this.dataForm.clFail,
              clC: this.dataForm.clC,
              clP: this.dataForm.clP,
              clFenleiId: this.dataForm.clFenleiId,
              clFileAddr: this.dataForm.clFileAddr,
              // clCreatePerson: this.dataForm.clCreatePerson,
              // clCreateDate: this.dataForm.clCreateDate,
              // clUpdatePerson: this.dataForm.clUpdatePerson,
              // clUpdateDate: this.dataForm.clUpdateDate
            })
          }).then(({ data }) => {
            if (data && data.code === 0) {
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
        if (!this.curveIdList[i].wcXishu) {
          if (!n[this.curveIdList[i].wcNo]) {
            //如果hash表中没有当前项
            const temp = {};
            n[this.curveIdList[i].wcNo] = true; //存入hash表
            r.push(this.curveIdList[i].wcNo); //把当前数组的当前项push到临时数组里面
          }
          if (this.curveIdList[i].wcNo === r[0]) {
            x.push(this.curveIdList[i].wcX);
          }
        }
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
            this.$set(temp, "name", this.curveIdList[i].wcNo);
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

    //临时点击事件
    hanleClick(){
      console.log("yyyyyyyyyyyyyyyyyy");
    }
  }
};
</script>
<style scoped>
  .chooseButton {
    text-align:center;
  }
</style>