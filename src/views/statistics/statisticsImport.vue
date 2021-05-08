<template>
  <div class="app-container">
    <div class="table-box">
      <el-form
        :model="queryParams"
        ref="queryForm"
        :inline="true"
        label-width="60px"
      >
        <el-form-item label="" >
          <el-upload
            action="invalidStr"
            class="upload-demo"
            :show-file-list="false"
            :http-request="upload"
          >
            <el-button size="small" type="primary">选择文件(单选)</el-button>
          </el-upload>
        </el-form-item>
        <el-form-item label="" >
          <span style="">{{ fileName }}</span>
        </el-form-item>
        <el-form-item label="品种" >
          <el-select
            v-model="queryParams.types"
            placeholder="请选择品种"
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
        <el-form-item label="类型" >
          <el-select
            v-model="queryParams.varieties"
            placeholder="请选择类型"
            clearable
            size="small"
          >
            <el-option
              v-for="dict in typesOptions"
              :key="dict.dictValue"
              :label="dict.dictLabel"
              :value="dict.dictValue"
            />
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" size="mini" @click="handleQuery"
            >导入</el-button
          >
        </el-form-item>
      </el-form>

      <el-table v-loading="loading" :data="postList">
        <el-table-column label="流水号" align="center" prop="postId" />
        <el-table-column label="文件" align="center" prop="postCode" />
        <el-table-column label="品种" align="center" prop="postName" />
        <el-table-column label="类型" align="center" prop="postSort" />
        <el-table-column label="导入数量" align="center" prop="postSort" />

        <el-table-column
          label="导入时间"
          align="center"
          prop="createTime"
          width="180"
        >
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="操作"
          align="center"
          class-name="small-padding fixed-width"
        >
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="text"
              @click="handleDelete(scope.row)"
              v-hasPermi="['system:post:remove']"
              >下载</el-button
            >
          </template>
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

    <!-- 第二个表格 -->
    <div>
      <el-form
        :model="queryParams2"
        ref="queryForm2"
        :inline="true"
        label-width="85px"
      >
        <el-form-item label="收获年度" prop="year" >
          <el-date-picker
            v-model="queryParams2.year"
            type="year"
            placeholder="选择年"
          >
          </el-date-picker>
        </el-form-item>

        <el-form-item label="库点地区"  prop="city" >
          <el-select
            v-model="queryParams2.city"
            placeholder="请选择库点地区"
            clearable
            size="small"
          >
            <el-option
              v-for="dict in citysOptions"
              :key="dict.dictValue"
              :label="dict.dictLabel"
              :value="dict.dictValue"
            />
          </el-select>
        </el-form-item>
        <el-form-item label="品种"  prop="varieties">
          <el-select 
            v-model="queryParams2.varieties"
            placeholder="请选择品种"
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

        <el-form-item label="储粮性质" prop="natures" >
          <el-select
            v-model="queryParams2.natures"
            placeholder="请选择储粮性质"
            clearable
            size="small"
          >
            <el-option
              v-for="dict in naturesOptions"
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
        </el-form-item>
      </el-form>

      <el-row :gutter="10" style="margin: 0px 10px 20px 0px">
        <el-col :span="1.5">
          <el-tag type="success">样品数量100份</el-tag>
        </el-col>
      </el-row>

      <el-table v-loading="loading" :data="postList">
        <el-table-column
          label="库点所在地行政区划"
          align="center"
          prop="postId"
        />
        <el-table-column label="检查库点" align="center" prop="postCode" />
        <el-table-column
          label="扦样仓房（货位）号"
          align="center"
          prop="postName"
        />
        <el-table-column label="品种" align="center" prop="postSort" />
        <el-table-column label="扦样区域" align="center" prop="status" />
        <el-table-column label="代表数量（吨）" align="center" prop="status" />
        <el-table-column label="产地" align="center" prop="status" />
        <el-table-column label="收获年度" align="center" prop="status" />
        <el-table-column label="扦样区域" align="center" prop="status" />
        <el-table-column label="储粮性质" align="center" prop="status" />
        <el-table-column label="入库时间" align="center" prop="status" />
        <el-table-column label="入库等级" align="center" prop="status" />

        <el-table-column
          label="操作"
          align="center"
          class-name="small-padding fixed-width"
        >
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="text"
              icon="el-icon-edit"
              @click="openInspect()"
              >检验参数</el-button
            >
            <el-button
              size="mini"
              type="text"
              icon="el-icon-delete"
              @click="handleDelete(scope.row)"
              v-hasPermi="['system:post:remove']"
              >删除</el-button
            >
          </template>
        </el-table-column>
      </el-table>

      <pagination
        v-show="total > 0"
        :total="total"
        :page.sync="queryParams.pageNum"
        :limit.sync="queryParams.pageSize"
        @pagination="getList"
      />
      <inspectItem ref="inspect" />
    </div>
  </div>
</template>

<script>
import { listPost, getPost, delPost, exportPost } from "@/api/system/post";
import inspectItem from "./inspectItem.vue";
export default {
  components: { inspectItem },
  name: "Post",
  data() {
    return {
      // 显示的文件名称
      fileName: "未选择文件",
      // 品种类型数组
      varietiesOptions: [
        {
          dictValue: "1",
          dictLabel: "稻谷",
        },
      ],
      // 类型
      typesOptions: [
        {
          dictValue: "1",
          dictLabel: "品质质量",
        },
      ],
      // 城市
      citysOptions: [
        {
          dictValue: "1",
          dictLabel: "南宁",
        },
      ],
      // 性质
      naturesOptions: [
        {
          dictValue: "1",
          dictLabel: "性质",
        },
      ],
      // 遮罩层
      loading: true,
      // 选中数组
      ids: [],
      // 非单个禁用
      single: true,
      // 非多个禁用
      multiple: true,
      // 总条数
      total: 0,
      // 岗位表格数据
      postList: [],
      // 弹出层标题
      title: "",
      // 是否显示弹出层
      open: false,
      // 状态数据字典
      statusOptions: [],
      // 查询参数
      queryParams: {
        pageNum: 1,
        pageSize: 10,
        filePath: "",
        types: undefined,
        varieties: undefined,
      },
      queryParams2: {
        pageNum: 1,
        pageSize: 10,
        types: undefined,
        varieties: undefined,
        year: undefined,
        city: undefined,
        natures:undefined
      },
      // 表单参数
      form: {},
    };
  },
  created() {
    this.getList();
  },
  methods: {
    openInspect() {
      this.$refs["inspect"].open();
    },
    upload(val) {
      this.fileName = val.file.name;
    },
    /** 查询岗位列表 */
    getList() {
      this.loading = true;
      listPost(this.queryParams).then((response) => {
        this.postList = response.rows;
        this.total = response.total;
        this.loading = false;
      });
    },
    // // 岗位状态字典翻译
    // statusFormat(row, column) {
    //   return this.selectDictLabel(this.statusOptions, row.status);
    // },

    // 表单重置
    reset() {
     
      this.resetForm("form");
    },
    /** 搜索按钮操作 */
    handleQuery() {
      this.queryParams.pageNum = 1;
      this.getList();
    },
    /** 重置按钮操作 */
    resetQuery() {
      this.resetForm("queryForm2");
      this.handleQuery();
    },

    /** 删除按钮操作 */
    handleDelete(row) {
      const postIds = row.postId || this.ids;
      this.$confirm(
        '是否确认删除岗位编号为"' + postIds + '"的数据项?',
        "警告",
        {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning",
        }
      )
        .then(function () {
          return delPost(postIds);
        })
        .then(() => {
          this.getList();
          this.msgSuccess("删除成功");
        })
        .catch(function () {});
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


<style lang="scss" scoped>
.table-box {
  margin-bottom: 80px;
}
</style>