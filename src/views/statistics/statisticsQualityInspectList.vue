<template>
  <div class="app-container">
    <el-form
      :model="queryParams"
      ref="queryForm"
      :inline="true"
      v-show="showSearch"
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
        <el-button type="primary" size="mini" @click="handleQuery"
          >导出</el-button
        >
      </el-form-item>
    </el-form>

    <el-table
      :span-method="cellMerge"
      border
      :data="tableData"
      style="width: 100%"
    >
      <el-table-column label="分性质分品种统计">
        <el-table-column prop="title" label=""> </el-table-column>
        <el-table-column prop="classification" label=""> </el-table-column>
        <el-table-column prop="detail" label=""> </el-table-column>
      </el-table-column>
      <el-table-column label="检查地区">
        <el-table-column prop="total" label="合计"> </el-table-column>
        <template> </template>
        <el-table-column
          v-for="(item, index) in addrArr"
          :key="index"
          :prop="item.key"
          :label="item.name"
        >
        </el-table-column>
        <!-- <el-table-column prop="amount1" label="1"> </el-table-column>
        <el-table-column prop="amount1" label="2"> </el-table-column>
        <el-table-column prop="amount1" label="3"> </el-table-column>
        <el-table-column prop="amount1" label="4"> </el-table-column> -->
      </el-table-column>
    </el-table>
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
      addrArr: [
        {
          name: "南宁",
          key: "nanNing",
        },
        {
          name: "百色",
          key: "baiSe",
        },
        {
          name: "天津",
          key: "tainJing",
        },
      ],
      spanArr: [],
      spanArr2: [],
      tableData: [],

      // 遮罩层
      loading: true,
      // 选中数组
      ids: [],
      // 非单个禁用
      single: true,
      // 非多个禁用
      multiple: true,
      // 显示搜索条件
      showSearch: true,
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
        postCode: undefined,
        postName: undefined,
        status: undefined,
        year: undefined,
      },
      // 表单参数
      form: {},
      // 表单校验
      rules: {
        postName: [
          { required: true, message: "岗位名称不能为空", trigger: "blur" },
        ],
        postCode: [
          { required: true, message: "岗位编码不能为空", trigger: "blur" },
        ],
        postSort: [
          { required: true, message: "岗位顺序不能为空", trigger: "blur" },
        ],
      },
    };
  },
  created() {
    let arr = [
      {
        name: "整体情况",
        key: "title",
        children: [
          {
            name: "合计",
            key: "classification",
            children: [
              {
                name: "检查企业个数1",
                key: "detail",
                children: [
                  {
                    key: "total",
                    name: "合计",
                    value: 3435,
                  },
                  {
                    key: "nanNing", // 地区编码
                    name: "南宁",
                    value: 1223243,
                  },
                  {
                    key: "baiSe", // 地区编码
                    name: "百色",
                    value: 1231423,
                  },
                  {
                    key: "tainJing", // 地区编码
                    name: "天津",
                    value: 125433,
                  },
                ],
              },
              {
                name: "检查企业个数",
                key: "detail",
                children: [
                  {
                    key: "total",
                    name: "合计",
                    value: 3435,
                  },
                  {
                    key: "nanNing", // 地区编码
                    name: "南宁",
                    value: 1223243,
                  },
                  {
                    key: "baiSe", // 地区编码
                    name: "百色",
                    value: 1231423,
                  },
                  {
                    key: "tainJing", // 地区编码
                    name: "天津",
                    value: 125433,
                  },
                ],
              },
              {
                name: "样品总数",
                key: "detail",
                children: [
                  {
                    key: "total",
                    name: "合计",
                    value: 3435,
                  },
                  {
                    key: "nanNing", // 地区编码
                    name: "南宁",
                    value: 1223243,
                  },
                  {
                    key: "baiSe", // 地区编码
                    name: "百色",
                    value: 1231423,
                  },
                  {
                    key: "tainJing", // 地区编码
                    name: "天津",
                    value: 125433,
                  },
                ],
              },
            ],
          },
          {
            name: "稻谷",
            key: "classification",
            children: [
              {
                name: "检查企业个数",
                key: "detail",
                children: [
                  {
                    key: "total",
                    name: "合计",
                    value: 3435,
                  },
                  {
                    key: "nanNing", // 地区编码
                    name: "南宁",
                    value: 1223243,
                  },
                  {
                    key: "baiSe", // 地区编码
                    name: "百色",
                    value: 1231423,
                  },
                  {
                    key: "tainJing", // 地区编码
                    name: "天津",
                    value: 125433,
                  },
                ],
              },
              {
                name: "样品总数",
                key: "detail",
                children: [
                  {
                    key: "total",
                    name: "合计",
                    value: 3435,
                  },
                  {
                    key: "nanNing", // 地区编码
                    name: "南宁",
                    value: 1223243,
                  },
                  {
                    key: "baiSe", // 地区编码
                    name: "百色",
                    value: 1231423,
                  },
                  {
                    key: "tainJing", // 地区编码
                    name: "天津",
                    value: 125433,
                  },
                ],
              },
            ],
          },
        ],
      },
      {
        name: "整体情况2",
        key: "title",
        children: [
          {
            name: "合计",
            key: "classification",
            children: [
              {
                name: "检查企业个数",
                key: "detail",
                children: [
                  {
                    key: "total",
                    name: "合计",
                    value: 3435,
                  },
                  {
                    key: "nanNing", // 地区编码
                    name: "南宁",
                    value: 1223243,
                  },
                  {
                    key: "baiSe", // 地区编码
                    name: "百色",
                    value: 1231423,
                  },
                  {
                    key: "tainJing", // 地区编码
                    name: "天津",
                    value: 125433,
                  },
                ],
              },
              {
                name: "样品总数",
                key: "detail",
                children: [
                  {
                    key: "total",
                    name: "合计",
                    value: 3435,
                  },
                  {
                    key: "nanNing", // 地区编码
                    name: "南宁",
                    value: 1223243,
                  },
                  {
                    key: "baiSe", // 地区编码
                    name: "百色",
                    value: 1231423,
                  },
                  {
                    key: "tainJing", // 地区编码
                    name: "天津",
                    value: 125433,
                  },
                ],
              },
            ],
          },
          {
            name: "稻谷",
            key: "classification",
            children: [
              {
                name: "检查企业个数",
                key: "detail",
                children: [
                  {
                    key: "total",
                    name: "合计",
                    value: 3435,
                  },
                  {
                    key: "nanNing", // 地区编码
                    name: "南宁",
                    value: 1223243,
                  },
                  {
                    key: "baiSe", // 地区编码
                    name: "百色",
                    value: 1231423,
                  },
                  {
                    key: "tainJing", // 地区编码
                    name: "天津",
                    value: 125433,
                  },
                ],
              },
              {
                name: "样品总数",
                key: "detail",
                children: [
                  {
                    key: "total",
                    name: "合计",
                    value: 3435,
                  },
                  {
                    key: "nanNing", // 地区编码
                    name: "南宁",
                    value: 1223243,
                  },
                  {
                    key: "baiSe", // 地区编码
                    name: "百色",
                    value: 1231423,
                  },
                  {
                    key: "tainJing", // 地区编码
                    name: "天津",
                    value: 125433,
                  },
                ],
              },
            ],
          },
        ],
      },
    ];

    let initArr = [];
    arr.forEach((item) => {
      item.children.forEach((item2) => {
        item2.children.forEach((item3) => {
          let obj = {
            [item.key]: item.name,
            [item2.key]: item2.name,
            [item3.key]: item3.name,
          };
          item3.children.forEach((item4) => {
            obj[item4.key] = item4.value;
          });
          initArr.push(obj);
        });
      });
    });

    this.tableData = initArr;
    this.getSpanArr(this.tableData); // 收集需要合并的咧
    // this.getList();
  },
  methods: {
    getSpanArr(data) {
      for (var i = 0; i < data.length; i++) {
        if (i === 0) {
          // 收集第一列数据
          this.spanArr.push(1);
          this.pos = 0;
          // 收集第二列数据

          this.spanArr2.push(1);
          this.pos2 = 0;
        } else {
          // 收集第一列数据，并判断当前元素与上一个元素是否相同
          if (data[i].title === data[i - 1].title) {
            this.spanArr[this.pos] += 1;
            this.spanArr.push(0);
          } else {
            this.spanArr.push(1);
            this.pos = i;
          }

          // 收集第二列数据，并判断当前元素与上一个元素是否相同
          if (data[i].classification === data[i - 1].classification) {
            this.spanArr2[this.pos2] += 1;
            this.spanArr2.push(0);
          } else {
            this.spanArr2.push(1);
            this.pos2 = i;
          }
        }
      }
    },
    cellMerge({ row, column, rowIndex, columnIndex }) {
      if (columnIndex === 0) {
        let _row = this.spanArr[rowIndex];
        let _col = _row > 0 ? 1 : 0;
        let obj = {
          rowspan: _row,
          colspan: _col,
        };
        return obj;
      }

      if (columnIndex === 1) {
        let _row = this.spanArr2[rowIndex];
        let _col = _row > 0 ? 1 : 0;
        let obj = {
          rowspan: _row,
          colspan: _col,
        };
        return obj;
      }
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

    // 取消按钮
    cancel() {
      this.open = false;
      this.reset();
    },
    // 表单重置
    reset() {
      this.form = {
        postId: undefined,
        postCode: undefined,
        postName: undefined,
        postSort: 0,
        status: "0",
        remark: undefined,
      };
      this.resetForm("form");
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