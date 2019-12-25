<template>
<div>
      <!-- <h3>父组件传给子组件的addrName:{{sendData}}</h3>
   -->

      <!-- <el-col :span="10"> -->
        <!-- <el-card class="mod-demo-echartst"> -->
        <div id="myChart" class="chart-box" :option="option"></div>
        <!-- </el-card> -->
      <!-- </el-col> -->
  
</div>
</template>

<script>
/* eslint-disable */
export default {
  name: "eCharts",
  props:["sendData"],
  data() {
    return {
      msg: "",
      id:'',
      option: {
        title: {
          text: "折线图堆叠"
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
            magicType: { show: true, type: ["line", "bar"] },
            restore: { show: true },
            saveAsImage: { show: true }
          }
        },
        //   option: Line_options,
        xAxis: {
          type: "category",
          boundaryGap: false,
          // data: ['A','B','C','D','E','F','G']
          data: ""
        },
        yAxis: {
          type: "value"
        },
        series: [
          {
            name: "A曲线",
            type: "line"
          },
          {
            name: "B曲线",
            type: "line"
          },
          {
            name: "C曲线",
            type: "line"
          }
        ]
      },
      infoList:[]
      
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.drawLine();
    })
  },
  methods: {
    drawLine() {

      // 基于准备好的dom，初始化echarts实例
      var myChart = this.$echarts.init(document.getElementById("myChart"));
      //  this.infoList = JSON.stringify(this.sendData)
     // console.log("========"+JSON.stringify(this.sendData[1].wcNo))
      const listXAxis = [];
        var n = {}, r = []; //n为hash表，r为临时数组
              for (var i = 0; i < this.sendData.length; i++) //遍历当前数组
              {
               // console.log(this.userList[i].name)
                if (!n[this.sendData[i].wcNo]) //如果hash表中没有当前项
                {
                  n[this.sendData[i].wcX] = true; //存入hash表
                  r.push(this.sendData[i].wcNo); //把当前数组的当前项push到临时数组里面
                }
              }
         this.option.legend.data=r
          console.log("id====="+r)
      // 绘制图表 
      //  this.option.xAxis.data = this.tempList
    // this.$http({
    //           url: this.$http.adornUrl(`/generator/tminfoaxis/info/${chartId}`),
    //           method: 'get',
    //           params: this.$http.adornParams()
    //         }).then(({data}) => {
    //           if (data && data.code === 0) {
    //             console.log("9999999999999")
             
    //             // this.dataForm.xaxis = data.tmInfoAxis.xaxis
    //             // this.dataForm.yaxis = data.tmInfoAxis.yaxis
    //             // this.dataForm.infoId = data.tmInfoAxis.infoId
    //           }
    //         })
      myChart.setOption(this.option);
    }
  }
};
</script>

<style lang="scss">
// .mod-demo-echartst {
//   > .el-alert {
//     margin-bottom: 10px;
//   }
//   > .el-row {
//     margin-top: -10px;
//     margin-bottom: -10px;
//     .el-col {
//       padding-top: 10px;
//       padding-bottom: 10px;
//     }
//   }
//   .chart-box {
//     min-height: 290px;
//   }
// }
.chart-box {
  min-height: 400px;
}
</style>