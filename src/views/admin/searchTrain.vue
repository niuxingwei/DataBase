<!--
 * @Author: 牛兴炜
 * @Date: 2020-06-25 08:14:58
 * @LastEditTime: 2020-06-30 10:58:35
 * @LastEditors: Please set LastEditors
 * @Description: 列车信息查询
 * @FilePath: \12306\src\views\admin\searchTrain.vue
 -->
<template>
  <div class="train">
    <div class="nav">
      <top-nav :showContent="showContent"></top-nav>
    </div>
    <br>
    <!-- 表格；data:显示的数据;stripe:是否为斑马纹table;highlight-current-row:是否要高亮当前行;border:是否带有纵向边框；v-loading:加载数据时显示 -->
    <el-table :row-class-name="tableRowClassName" :data="queryTrainTable" border v-loading="dataListLoading">
      <el-table-column header-align="center" align="center" width="55" type="index" label="  "></el-table-column>
      <el-table-column prop="SStation" header-align="center" align="center" label="始发站"></el-table-column>
      <el-table-column prop="EStation" header-align="center" align="center" label="终点站"></el-table-column>
      <el-table-column prop="TrainNumber" header-align="center" align="center" label="列车号"></el-table-column>
      <el-table-column prop="BTime" header-align="center" align="center" label="开车时间">
        <template slot-scope="scope">
          <svg-icon icon-class="time"></svg-icon>
          <span style="margin-left: 5px">{{ scope.row.BTime }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="ATIME" header-align="center" align="center" label="到站时间">
        <template slot-scope="scope">
          <svg-icon icon-class="time"></svg-icon>
          <span style="margin-left: 5px">{{ scope.row.ATIME }}</span>
        </template>
      </el-table-column>
    </el-table>
    <br>
    <br>
    <!-- 分页；total：总条目数；current-page：当前页数；page-size：每页显示条目个数;current-change:当前页改变时触发-->
    <div style="text-align:right">
      <el-pagination @current-change="currentChangeHandle()" :total="totalPage" :current-page="pageIndex" :page-size="pageSize" layout="total,prev, pager, next, jumper">
      </el-pagination>
    </div>
  </div>
</template>
<script>
import TopNav from '@/components/common/topNav/index'
export default {

  data () {
    return {
      //顶部导航
      showContent: {
        showBack: false,
        titleContent: '车票查询结果'
      },
      // 列车查询表单
      queryTrainTable: [],
      // 分页
      totalPage: 4,
      pageIndex: 1,
      pageSize: 10
    }
  },
  components: {
    TopNav
  },
  created () {
    this.getDataList()
  },

  methods: {
    /**
       * @description: 车票信息查询
       * @param {type} 
       * @return: 车站列表信息
       */
    getDataList () {
      this.dataListLoading = true
      // 获取数据，最后要将dataListLoading设置为false
      // alert("数据已获取！")
      let queryTrainTableTem = []// 将符合的数据存放进去
      this.$route.query.SelectTable.forEach(temCurrent => {
        queryTrainTableTem.push({
          SStation: temCurrent.SStation,
          EStation: temCurrent.EStation,
          BTime: temCurrent.STime,
          ATIME: temCurrent.ETime,
          TrainNumber: temCurrent.TrainNUmber,
        })
      })
      this.queryTrainTable = queryTrainTableTem
      // this.queryTrainTable = [
      //   { SStation: '北京西', EStation: '十堰', TrainNumber: 'K279', BTime: '2019-10-01 14:23:12', ATIME: '2019-10-03 14:23:12' },
      //   { SStation: '郑州', EStation: '平顶山', TrainNumber: 'K229', BTime: '2019-12-01 14:23:12', ATIME: '2019-11-13 14:23:12' }
      // ]
      this.dataListLoading = false
    },
    /**
     * @description: 表格数据着色，便于观察
     * @param {type} 
     * @return: 
     */
    tableRowClassName ({ row, rowIndex }) {
      if (rowIndex % 2 === 1) {
        return 'warning-row';
      } else if (rowIndex % 2 === 0) {
        return 'success-row';
      }
      return '';
    },
  }
}
</script>

<style  scoped>
.nav {
  background-color: #1fcca9;
}
.warning-row {
  background: green;
}
.success-row {
  background: green;
}
</style>