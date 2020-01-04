<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()" >
      <el-form-item>
        <el-input v-model="dataForm.clNo" placeholder="材料编号" :style="{width:'110px'}" class="inputCls"  clearable @input="getDataList()"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.clName" placeholder="材料名称" :style="{width:'110px'}" class="inputCls"  clearable @input="getDataList()"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.clPaihao" placeholder="材料牌号" :style="{width:'110px'}" class="inputCls" clearable @input="getDataList()"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.clWendu" placeholder="温度" :style="{width:'110px'}" class="inputCls" clearable @input="getDataList()"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.clChangjia" placeholder="厂家" class="inputCls" clearable @input="getDataList()"></el-input>
      </el-form-item>
      <el-form-item>
        <!-- <el-input v-model="dataForm.key" placeholder="分类" clearable></el-input> -->
        <el-select v-model="dataForm.clFenleiId" placeholder="材料类型"  class="inputCls" @change="getDataList()">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <!-- <el-button @click="getDataList()">查询</el-button> -->
        <el-button type="info" style="margin-left: 8px" @click="clearHandle" icon="redo">重置</el-button>
        <el-button v-if="isAuth('cailiao:wisdplatcailiao:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>
        <el-button v-if="isAuth('cailiao:wisdplatcailiao:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">删除</el-button>
        <el-button type="success" @click="exportHandle()"   :disabled="dataListSelections.length<=0 || dataListSelections.length>1">导出</el-button>

      </el-form-item>
    </el-form>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column>
      <!-- <el-table-column
        prop="clId"
        header-align="center"
        align="center"
        label="主键ID">
      </el-table-column> -->
      <el-table-column
        prop="clNo"
        header-align="center"
        align="center"
        label="材料编号"
        width="200%">
      </el-table-column>
      <el-table-column
        prop="clName"
        header-align="center"
        align="center"
        label="材料名称">
      </el-table-column>
      <el-table-column
        prop="clPaihao"
        header-align="center"
        align="center"
        label="材料牌号">
      </el-table-column>
      <el-table-column
        prop="clWendu"
        header-align="center"
        align="center"
        label="温度">
      </el-table-column>
      <el-table-column
        prop="clChangjia"
        header-align="center"
        align="center"
        label="厂家">
      </el-table-column>
      <el-table-column
        prop="clFenleiId"
        header-align="center"
        align="center"
        label="分类">
      </el-table-column>
      <el-table-column
        prop="clMidu"
        header-align="center"
        align="center"
        label="材料密度(t/mm3)">
      </el-table-column>
      <el-table-column
        prop="clBosongbi"
        header-align="center"
        align="center"
        label="泊松比">
      </el-table-column>
      <el-table-column
        prop="clMoliang"
        header-align="center"
        align="center"
        label="弹性模量(Mpa)">
      </el-table-column>


      <!-- <el-table-column
        prop="clFileAddr"
        header-align="center"
        align="center"
        label="Excel文件地址">
      </el-table-column>
      <el-table-column
        prop="clCreatePerson"
        header-align="center"
        align="center"
        label="创建人">
      </el-table-column>
      <el-table-column
        prop="clCreateDate"
        header-align="center"
        align="center"
        label="创建时间">
      </el-table-column>
      <el-table-column
        prop="clUpdatePerson"
        header-align="center"
        align="center"
        label="修改人">
      </el-table-column>
      <el-table-column
        prop="clUpdateDate"
        header-align="center"
        align="center"
        label="修改时间">
      </el-table-column> -->
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.clId,'预览')">预览</el-button>
          <el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.clId)">修改</el-button>
          <el-button type="text" size="small" @click="deleteHandle(scope.row.clId)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 弹窗, 新增 / 修改 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
    <export-cailiao v-if="exportVisible" ref="exportCailiao"></export-cailiao>
  </div>
</template>

<script>
  /* eslint-disable */
  import AddOrUpdate from './wisdplatcailiao-add-or-update'
  import ExportCailiao from './wisdplat-export'
  import ElButton from "element-ui/packages/button/src/button";
  export default {
    data () {
      return {
        dataForm: {
          key: ''
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false,
        exportVisible:false,
        options: [
         {
          value: '金属',
          label: '金属'
        }, {
          value: '非金属',
          label: '非金属'
        },{
          value:'',
          label:'不限'
        }],
        value:'',
      }
    },
    components: {
      ElButton,
      AddOrUpdate,
      ExportCailiao
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/cailiao/wisdplatcailiao/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'limit': this.pageSize,
            'key': this.dataForm
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
          this.dataList = data.page.list
          this.totalPage = data.page.totalCount
        } else {
          this.dataList = []
          this.totalPage = 0
        }
        this.dataListLoading = false
      })
      },
      // 每页数
      sizeChangeHandle (val) {
        this.pageSize = val
        this.pageIndex = 1
        this.getDataList()
      },
      // 当前页
      currentChangeHandle (val) {
        this.pageIndex = val
        this.getDataList()
      },
      // 多选
      selectionChangeHandle (val) {
        this.dataListSelections = val
      },
      // 新增 / 修改
      addOrUpdateHandle (id,str) {
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init(id,str)
      })
      },
      //导出
      exportHandle (id){
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.clId
        })
        //调用dialog
        this.exportVisible=true
        this.$nextTick(() => {
          this.$refs.exportCailiao.init(ids)
         })
      },
      // 删除
      deleteHandle (id) {
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.clId
        })
        // this.$confirm(`确定对[id=${ids.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          this.$confirm(`确定对选择的数据进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
          url: this.$http.adornUrl('/cailiao/wisdplatcailiao/delete'),
          method: 'post',
          data: this.$http.adornData(ids, false)
        }).then(({data}) => {
          if (data && data.code === 0) {
          this.$message({
            message: '操作成功',
            type: 'success',
            duration: 1500,
            onClose: () => {
            this.getDataList()
        }
        })
        } else {
          this.$message.error(data.msg)
        }
      })
      })
      },
      //重置事情
      clearHandle() {
        this.dataForm = {}
        this.getDataList()
      },
    }
  }
</script>
<style>
  .inputCls{
     width:160px;
  }

.el-table th > .cell {
    position: relative;
    word-wrap: normal;
    text-overflow: ellipsis;
    vertical-align: middle;
    width: 100%;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    color: 
    white;
}
.el-table__header th, .el-table__header tr {

background-color: #17B3A3;

color: black;

}
a {
    color: 
    #373737;
    text-decoration: none;
}
</style>
