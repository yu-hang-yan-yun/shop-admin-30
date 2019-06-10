<template>
<div>
<el-row type="flex" justify="space-between">
  <div>
    <el-button>新增</el-button>
    <el-button type="danger" @click="handleDeleteMore">删除
    </el-button>
  </div>
  <!-- 搜索框 -->
<div class="input-search">
  <el-input placeholder="请输入内容" v-model="searchValue" class="input-with-select">
     <el-button slot="append" icon="el-icon-search" @click="hanleSearch">
     </el-button>
  </el-input>
</div>
</el-row>

<!-- //表格数据 -->
<el-table
    ref="multipleTable"
    :data="tableData"
    tooltip-effect="dark"
    style="width: 100%"
    @selection-change="handleSelectionChange">

    <el-table-column
      type="selection"
      width="55">
    </el-table-column>

    <el-table-column
      label="标题"
      width="200">
     <template slot-scope="scope">
       <el-row type="flex" align="middle">
          {{ scope.row.date }}
       </el-row>
     </template>
    </el-table-column>

    <el-table-column
      prop="categoryname"
      label="类型"
      width="200">
    </el-table-column>

    <el-table-column
      prop="sell_price"
      label="价格"
      width="200">
    </el-table-column>

<!-- //操作 -->
<el-table-column label="操作">
  <template slot-scope="scope">
    <el-button
      size="mini"
      @click="handleEdit(scope.row)">编辑
    </el-button>
    <el-button
      size="mini"
      type="danger"
      @click="handleDelete(scope.row)">删除
    </el-button>
  </template>
</el-table-column>
</el-table>

<!-- //分页 -->
<el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pageIndex"
      :page-sizes="[5, 10, 15, 20]"
      :page-size="5"
      layout="total, sizes, prev, pager, next, jumper"
      :total="400">
</el-pagination>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        tableData: [],
        //选中的商品
        aelectGoods:[],

        //搜索关键字
        searchValue:"",

        //默认页面，随页面的切换变化
        pageIndex:1,

        //页面显示条数
        pageSize:5,

        //总条数
        total:0
      }
    },
  methods: {
    getList(){
        this.$axios({
            url:`http://localhost:8899/admin/goods/getlist?pageIndex=${this.pageIndex}&pageSize=${this.pageSize}&searchvalue=${this.searchValue}`,
            method:"GET"
        }).then(res => {
            var data =res.data;
            // 商品列表数据
            this.tableData=data.message;
            //总条数
            this.total=data.totalcount;
        })
    },
      
    handleSelectionChange(val) {
        this.selectGoods = val;
    },
    
    //编辑商品
    handleEdit(goods) {
        console.log(goods);
    },

  
    
    //删除商品
    handleDelete(index, row) {
        console.log(index, row);
    },
    //分页
    handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
        },
    handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
    }
  },
  mounted(){
      this.getList();
  }
}
</script>

<style>
.el-table{
    margin-top: 20px;
}
.el-pagination{
    margin-top: 10px;
}
.input-search{
    width: 500px;
}
 .input-with-select .el-input-group__prepend {
    background-color: #fff;
  }
</style>
