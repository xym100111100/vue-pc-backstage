<template>
  <div class="app-container">
    <el-form
      :model="queryParams"
      ref="queryForm"
      :inline="true"
      label-width="68px"
    >
      <el-form-item label="收获年度" prop="year">
        <el-date-picker
          v-model="queryParams.year"
          type="year"
          placeholder="选择年"
        >
        </el-date-picker>
      </el-form-item>

      <el-form-item label="品种" prop="varieties">
        <el-select
          v-model="queryParams.varieties"
          placeholder="请输入品种"
          clearable
          size="small"
        >
          <el-option
            v-for="dict in varietiesOptions"
            :key="dict.dictValue"
            :label="dict.dictLabel"
            :value="dict.dictValue"
          />
        </el-select>
      </el-form-item>

      <el-form-item>
        <el-button
          type="primary"
          icon="el-icon-search"
          size="mini"
          @click="handleQuery"
          >搜索</el-button
        >
        <el-button icon="el-icon-refresh" size="mini" @click="resetQuery"
          >重置</el-button
        >
        <el-button
          type="primary"
          icon="download"
          size="mini"
          @click="handleExport"
          >导出</el-button
        >
      </el-form-item>
    </el-form>

    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="createBy" label="检查地区或库点">
      </el-table-column>
      <el-table-column prop="createBy" label="样品编号"> </el-table-column>
      <el-table-column prop="createBy" label="扦祥仓房(活拉)号">
      </el-table-column>
      <el-table-column prop="createBy" label="粮食数量"> </el-table-column>
      <el-table-column prop="createBy" label="扦祥区域"> </el-table-column>
      <el-table-column prop="createBy" label="代表数量"> </el-table-column>
      <el-table-column prop="createBy" label="储粮性质"> </el-table-column>
      <el-table-column prop="createBy" label="粮食品种"> </el-table-column>
      <el-table-column prop="createBy" label="产地"> </el-table-column>
      <el-table-column prop="createBy" label="收获年度"> </el-table-column>
      <el-table-column prop="createBy" label="入库时间"> </el-table-column>
      <el-table-column label="不合格项目1">
        <el-table-column prop="createBy" label="项目名称"></el-table-column>
        <el-table-column prop="createBy" label="复核结果"> </el-table-column>
      </el-table-column>
      <el-table-column label="不合格项目2">
        <el-table-column prop="createBy" label="项目名称"></el-table-column>
        <el-table-column prop="createBy" label="复核结果"> </el-table-column>
      </el-table-column>
      <el-table-column label="不合格项目3">
        <el-table-column prop="createBy" label="项目名称"></el-table-column>
        <el-table-column prop="createBy" label="复核结果"> </el-table-column>
      </el-table-column>
    </el-table>
    <pagination
      v-show="total > 0"
      :total="total"
      :page.sync="queryParams.pageNum"
      :limit.sync="queryParams.pageSize"
      @pagination="getList"
    />
  </div>
</template>

<script>
import {
  listPost,
  getPost,
  delPost,
  addPost,
  updatePost,
  exportPost,
} from "@/api/system/post";

export default {
  name: "Post",
  data() {
    return {
      tableData: [
        {
          date: "2016-05-03",
          name: "王小虎",
          province: "上海",
          city: "普陀区",
          address: "上海市普陀区金沙江路 1518 弄",
          zip: 200333,
        },
      ],
      // 品种类型数组
      varietiesOptions: [
        {
          dictValue: "1",
          dictLabel: "稻谷",
        },
      ],
      // 总条数
      total: 0,
      // 岗位表格数据
      tableData: [],

      // 查询参数
      queryParams: {
        pageNum: 1,
        pageSize: 10,
        yaer: undefined,
        varieties: undefined,
      },
    };
  },
  created() {
    this.getList();
  },
  methods: {
    /** 查询岗位列表 */
    getList() {
      this.loading = true;
      listPost(this.queryParams).then((response) => {
        this.tableData = response.rows;
        this.total = response.total;
        this.loading = false;
      });
    },
    /** 搜索按钮操作 */
    handleQuery() {
      this.queryParams.pageNum = 1;
      this.getList();
    },
    /** 重置按钮操作 */
    resetQuery() {
      this.resetForm("queryForm");
      this.handleQuery();
    },

    /** 导出按钮操作 */
    handleExport() {
      const queryParams = this.queryParams;
      this.$confirm("是否确认导出所有岗位数据项?", "警告", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(function () {
          return exportPost(queryParams);
        })
        .then((response) => {
          this.download(response.msg);
        })
        .catch(function () {});
    },
  },
};
</script>