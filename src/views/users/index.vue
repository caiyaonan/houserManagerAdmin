<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input v-model="listQuery.account" placeholder="用户账号" style="width: 160px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-select v-model="listQuery.status" placeholder="状态" clearable style="margin-left: 10px;width: 90px" class="filter-item">
        <el-option v-for="(item,key) in statusOptions" :key="item" :label="item" :value="key" />
      </el-select>
      <el-button class="filter-item" type="primary" icon="el-icon-search" style="margin-left: 10px;" @click="handleFilter">
        搜索
      </el-button>
      <el-button class="filter-item" type="primary" icon="el-icon-search" style="margin-left: 10px;" @click="handleFilter">
        添加用户
      </el-button>
    </div>
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
      @sort-change="sortChange"
    >
      <el-table-column label="用户ID" prop="id" sortable="custom" align="center" width="95">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column>
      <el-table-column label="用户账号" width="160" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.account }}</span>
        </template>
      </el-table-column>
      <el-table-column label="用户类型" width="160" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.account }}</span>
        </template>
      </el-table-column>
      <el-table-column label="用户电话" width="160" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.account }}</span>
        </template>
      </el-table-column>
      <el-table-column label="用户邮箱" width="160" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.account }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" prop="created_at" label="创建时间" width="200">
        <template slot-scope="{row}">
          <i class="el-icon-time" />
          <span>{{ row.created_at }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center">
        <!-- <template slot-scope="{row}">
          <el-button v-if="row.status!=1" type="success" size="mini" @click="handleModifyStatus(row)">
            启用
          </el-button>
          <el-button v-if="row.status!=0" type="danger" size="mini" @click="handleModifyStatus(row)">
            禁用
          </el-button>
        </template> -->
      </el-table-column>
    </el-table>

    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="fetchData" />
  </div>
</template>

<script>
import { getList } from '@/api/admins'
import Pagination from '@/components/Pagination'

export default {
  components: { Pagination },
  filters: {
    statusFilter(status) {
      const statusMap = ['danger', 'success']
      return statusMap[status]
    },
    statusMeanFilter(status) {
      const statusMap = ['禁用', '正常']
      return statusMap[status]
    }
  },
  data() {
    return {
      list: null,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 20,
        sort: '-id',
        account: undefined,
        status: undefined
      },
      total: 0,
      statusOptions: ['禁用', '正常']
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    sortChange(data) {
      const { prop, order } = data
      if (order === 'ascending') {
        this.listQuery.sort = '+' + prop
      } else if (order === 'descending') {
        this.listQuery.sort = '-' + prop
      } else {
        this.listQuery.sort = '-id'
      }
      this.handleFilter()
    },
    handleFilter() {
      this.listQuery.page = 1
      this.fetchData()
    },
    fetchData() {
      // this.listLoading = true
      // getList(this.listQuery).then(response => {
        // this.list = response.data.items
        // this.total = response.data.total
        this.listLoading = false
      // })
    },
    handleModifyStatus(row) {
      var params = {
        id: row.id
      }
      switch (row.status) {
        case 0:
          params.status = 1
          break
        case 1:
          params.status = 0
          break
      }
      modifyStatus(params).then((res) => {
        this.$message({
          message: res.msg,
          type: 'success'
        })
        row.status = params.status
      })
    }
  }
}
</script>
