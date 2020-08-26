<template>
  <div class="app-container">
    <el-table
      ref="multipleTable"
      :data="list"
      tooltip-effect="dark"
      style="width: 100%"
      border
      stripe
      @selection-change="handleSelectionChange">
      <el-table-column
        type="selection"
        width="55">
      </el-table-column>
      <el-table-column
        label="id"
        width="120">
        <template slot-scope="scope">{{ scope.row.id }}</template>
      </el-table-column>
      <el-table-column
        prop="name"
        label="用户昵称"
        width="120">
      </el-table-column>
      <el-table-column
        prop="message"
        label="内容"
        show-overflow-tooltip>
      </el-table-column>
      <el-table-column
        prop="createTime"
        label="发布时间"
        width="120">
      </el-table-column>
      <el-table-column
        label="树洞显示状态"
        width="120">
        <template slot-scope="scope">
          <el-switch
            v-model="scope.row.status"
            active-text="显示"
            inactive-text="关闭"
            active-value="1"
            inactive-value="0">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column
        label="回应显示状态"
        width="120">
        <template slot-scope="scope">
          <el-switch
            v-model="scope.row.commentStatus"
            active-text="显示"
            inactive-text="关闭"
            active-value="1"
            inactive-value="0">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column
        prop="reponseNum"
        label="回复数量"
        width="120">
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)">详情</el-button>
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
      layout="total, sizes, prev, pager, next, jumper"
    ></el-pagination>
  </div>
</template>

<script>
import { getList } from '@/api/table'

export default {
  data() {
    return {
      pageIndex: 1,
      pageSize: 10,
      totalPage: 0,
     list:[],
      multipleSelection: []
    }
  },
created() {
  this.$http({
    url: this.$http.adornUrl('/message/list'),
    method: 'get',
    params: this.$http.adornParams({
      page: this.pageIndex,
      limit: this.pageSize
    })
  }).then(res=>{
    this.list=res.data.data.data.records
    console.log(this.list)
  })
},
  methods: {
    sizeChangeHandle(val) {
      this.pageSize = val;
      this.pageIndex = 1;
      this.getDataList();
    },
    // 当前页
    currentChangeHandle(val) {
      this.pageIndex = val;
      this.getDataList();
    },
    toggleSelection(rows) {
      if (rows) {
        rows.forEach(row => {
          this.$refs.multipleTable.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable.clearSelection();
      }
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    }
  }
}

</script>
