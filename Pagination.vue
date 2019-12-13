<start --->
父组件引入：
<pagination :pagination-data="paginationData"/>
paginationData: {
  page: 1,
  pageSize: 20,
  pageSizes: [10, 20, 50, 100],
  currentPage: 1,
  total: 0,
  // 每页条数
  handleSizeChange: val => {
    this.paginationData.page = 1
    this.paginationData.pageSize = val
    this.paginationData.currentPage = 1
    this.getPolicyListInfo()
  },
  handleCurrentChange: val => {
    this.paginationData.page = val
    this.paginationData.currentPage = val
    this.getPolicyListInfo()
  }
}
<end --->
  <template>
  <!-- 分页 -->
  <div class="pagination">
    <el-pagination
      :page-sizes="paginationData.pageSizes"
      :page-size="paginationData.pageSize"
      :current-page="paginationData.currentPage"
      :total="paginationData.total"
      layout="prev, pager, next, sizes, jumper"
      @size-change="paginationData.handleSizeChange"
      @current-change="paginationData.handleCurrentChange">
    </el-pagination>
  </div>
</template>

<script>

export default {
  props: {
    paginationData: {
      type: Object,
      default: Object
    }
  },
  data() {
    return {
    }
  },
  watch: {
  },
  created() {
  },
  methods: {
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .pagination {
    width: 100%;
    height: 30px;
    line-height: 30px;
    margin-top: 30px;
    text-align: right;
  }
</style>
