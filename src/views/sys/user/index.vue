<template> 
  <div class="app-container">
    <el-card class="filter-container" shadow="never">
      <div>
        <i class="el-icon-search"></i>
        <span>筛选搜索</span>
      </div>
      <div style="margin-top: 15px">
        <el-form :inline="true" :model="listQuery" size="small" label-width="140px">
          <el-form-item label="帐号：">
            <el-input style="width: 203px" v-model="listQuery.productName" placeholder="商品名称"></el-input>
          </el-form-item>
          <el-form-item label="商家类型：" v-for="item in type" :key="item.value">
            <el-select placeholder="请选择" v-model="listQuery.typeOption">
              <el-option :label="item.label" :value="item.value"></el-option>
            </el-select>
          </el-form-item>
          <el-button
            @click="handleSearchList()"
            type="primary"
            size="small"
            icon="el-icon-search">
            搜索
          </el-button>
          <el-button
            @click="handleResetSearch()"
            size="small">
            重置
          </el-button>
        </el-form>
      </div>
    </el-card>
    <el-card class="operate-container" shadow="never">
      <i class="el-icon-tickets"></i>
      <span>管理员列表</span>
      <span class="btn-add">
        <el-button
          @click="handleAddProduct()"
          size="mini"
          icon="el-icon-plus"
          type="primary">
          新建
        </el-button>
        <el-button
          @click="handleDelete(scope.$index, scope.row)"
          size="mini"
          icon="el-icon-delete"
          type="danger">
          删除
        </el-button>
      </span>
    </el-card>
    <div class="table-container">
      <el-table ref="productTable"
                :data="list"
                style="width: 100%"
                @selection-change="handleSelectionChange"
                v-loading="listLoading"
                border>
        <el-table-column type="selection" width="60" align="center"></el-table-column>
        <el-table-column type="index" label="序号" width="100" align="center"></el-table-column>
        <el-table-column label="账号" width="120" align="center">
          <template slot-scope="scope"><img style="height: 80px" :src="scope.row.pic"></template>
        </el-table-column>
        <el-table-column label="角色" align="center">
          <template slot-scope="scope">
            <p>品牌：{{scope.row.brandName}}</p>
          </template>
        </el-table-column>
        <el-table-column label="商家类型" align="center">
          <template slot-scope="scope">
            <p>价格：￥{{scope.row.price}}</p>
          </template>
        </el-table-column>
        <el-table-column label="状态" align="center">
          <template slot-scope="scope">{{scope.row.sort}}</template>
        </el-table-column>
        <el-table-column label="创建时间" align="center">
          <template slot-scope="scope">{{scope.row.sale}}</template>
        </el-table-column>
        <el-table-column label="操作" align="center">
          <template slot-scope="scope">
            <p>
              <el-button
                size="mini"
                type="primary"
                plain
                @click="handleUpdateProduct(scope.$index, scope.row)">查看编辑
              </el-button>
            </p>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <div class="pagination-container">
      <el-pagination
        background
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        layout="total, sizes,prev, pager, next,jumper"
        :page-size="listQuery.pageSize"
        :page-sizes="[5,10,15]"
        :current-page.sync="listQuery.pageNum"
        :total="total">
      </el-pagination>
    </div>
  </div>
</template>
<script>
  import { fetchList, updateDeleteStatus } from '@/api/product'

  const defaultListQuery = {
    productName: null,
    pageNum: 1,
    pageSize: 5,
    typeOption:''
  };
  export default {
    name: "user",
    data() {
      return {
        listQuery: Object.assign({}, defaultListQuery),
        list: null,
        total: null,
        listLoading: true,
        multipleSelection: [],
        type:[
          {value: 0,label: '勇艺达'}
        ],

      }
    },
    created() {
      this.getList();
    },
    watch: {},
    filters: {},
    methods: {
      getList() {
        this.listLoading = true;
        fetchList(this.listQuery).then(response => {
          this.listLoading = false;
          this.list = response.data.list;
          this.total = response.data.total;
        });
      },

      handleSearchList() {
        this.listQuery.pageNum = 1;
        this.getList();
      },

      handleAddProduct() {
        this.$router.push({path: '/pms/addProduct'});
      },

      handleSizeChange(val) {
        this.listQuery.pageNum = 1;
        this.listQuery.pageSize = val;
        this.getList();
      },
      handleCurrentChange(val) {
        this.listQuery.pageNum = val;
        this.getList();
      },
      handleSelectionChange(val) {
        this.multipleSelection = val;
      },
      handleResetSearch() {
        this.listQuery = Object.assign({}, defaultListQuery);
      },

      handleDelete(index, row) {
        this.$confirm('是否要进行删除操作?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          let ids = [];
          ids.push(row.id);
          this.updateDeleteStatus(1, ids);
        });
      },

      handleUpdateProduct(index, row) {
        this.$router.push({path: '/pms/updateProduct', query: {id: row.id}});
      },

    }
  }
</script>
<style></style>



