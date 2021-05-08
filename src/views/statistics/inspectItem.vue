<template>
  <el-dialog title="提示" :visible.sync="dialogVisible" width="60%">
    <div v-if="type == 1">
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>质量和储存品质检验结果</span>
        </div>

        <el-table border :data="tableData" style="width: 100%">
          <el-table-column prop="date" label="出糙率"> </el-table-column>
          <el-table-column prop="date" label="整精米率"> </el-table-column>
          <el-table-column prop="date" label="杂质"> </el-table-column>
          <el-table-column prop="date" label="水分"> </el-table-column>
          <el-table-column prop="date" label="黄粒米"> </el-table-column>
          <el-table-column prop="date" label="色泽气味"> </el-table-column>
          <el-table-column prop="date" label="检验等级"> </el-table-column>
          <el-table-column prop="date" label="结果判定(达标/不达标)">
          </el-table-column>
          <el-table-column prop="date" label="脂肪酸值"> </el-table-column>
          <el-table-column prop="date" label="品尝评分值"> </el-table-column>
          <el-table-column prop="date" label="色泽气味"> </el-table-column>
          <el-table-column
            prop="date"
            label="结果判定(宜存/轻度不宜存/重度不宜存)"
          >
          </el-table-column>
        </el-table>
      </el-card>
      <el-card class="box-card" style="margin-top: 20px">
        <div slot="header" class="clearfix">
          <span>食品安全检验项目及结果</span>
        </div>

        <el-table :data="tableData" style="width: 100%">
          <el-table-column prop="date" label="黄曲霉毒素B1"> </el-table-column>
          <el-table-column label="重金属">
            <el-table-column prop="date" label="铅"></el-table-column>
            <el-table-column prop="date" label="镉"> </el-table-column>
            <el-table-column prop="date" label="汞"> </el-table-column>
            <el-table-column prop="date" label="无机砷"> </el-table-column>
          </el-table-column>
          <el-table-column prop="date" label="结果判定（合格/不合格）">
          </el-table-column>
          <el-table-column label="不合格项目1">
            <el-table-column prop="date" label="项目名称"></el-table-column>
            <el-table-column prop="date" label="复核结果"> </el-table-column>
          </el-table-column>
          <el-table-column label="不合格项目2">
            <el-table-column prop="date" label="项目名称"></el-table-column>
            <el-table-column prop="date" label="复核结果"> </el-table-column>
          </el-table-column>
          <el-table-column label="不合格项目3">
            <el-table-column prop="date" label="项目名称"></el-table-column>
            <el-table-column prop="date" label="复核结果"> </el-table-column>
          </el-table-column>
          <el-table-column prop="date" label="加工等级"> </el-table-column>
        </el-table>
      </el-card>
    </div>
    <div v-else>
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>质量和储存品质检验项目及结果</span>
        </div>

        <el-table border :data="tableData" style="width: 100%">
          <el-table-column prop="date" label="容重"> </el-table-column>
          <el-table-column prop="date" label="不完善粒（总量）">
          </el-table-column>
          <el-table-column label="其中">
            <el-table-column prop="date" label="生霉粒"> </el-table-column>
            <el-table-column prop="date" label="生芽粒"> </el-table-column>
          </el-table-column>
          <el-table-column prop="date" label="杂质总量"> </el-table-column>
          <el-table-column prop="date" label="水分"> </el-table-column>
          <el-table-column prop="date" label="色泽气味"> </el-table-column>
          <el-table-column prop="date" label="检验等级"> </el-table-column>
          <el-table-column prop="date" label="结果判定(达标/不达标)">
          </el-table-column>
          <el-table-column prop="date" label="面筋吸水量"> </el-table-column>
          <el-table-column prop="date" label="品尝评分值"> </el-table-column>
          <el-table-column prop="date" label="色泽气味"> </el-table-column>
          <el-table-column
            prop="date"
            label="结果判定(宜存/轻度不宜存/重度不宜存)"
          >
          </el-table-column>
        </el-table>
      </el-card>
      <el-card class="box-card" style="margin-top: 20px">
        <div slot="header" class="clearfix">
          <span>食品安全检验项目及结果</span>
        </div>

        <el-table :data="tableData" style="width: 100%">
          <el-table-column label="真菌毒素">
            <el-table-column prop="date" label="呕吐毒素"></el-table-column>
            <el-table-column prop="date" label="玉米赤霉烯酮"> </el-table-column>
          </el-table-column>
          <el-table-column label="重金属">
            <el-table-column prop="date" label="铅"></el-table-column>
            <el-table-column prop="date" label="镉"> </el-table-column>
            <el-table-column prop="date" label="汞"> </el-table-column>
            <el-table-column prop="date" label="无机砷"> </el-table-column>
          </el-table-column>
          <el-table-column prop="date" label="结果判定（合格/不合格）">
          </el-table-column>
          <el-table-column label="不合格项目1">
            <el-table-column prop="date" label="项目名称"></el-table-column>
            <el-table-column prop="date" label="复核结果"> </el-table-column>
          </el-table-column>
          <el-table-column label="不合格项目2">
            <el-table-column prop="date" label="项目名称"></el-table-column>
            <el-table-column prop="date" label="复核结果"> </el-table-column>
          </el-table-column>
          <el-table-column label="不合格项目3">
            <el-table-column prop="date" label="项目名称"></el-table-column>
            <el-table-column prop="date" label="复核结果"> </el-table-column>
          </el-table-column>
          <el-table-column prop="date" label="加工等级"> </el-table-column>
        </el-table>
      </el-card>
    </div>

    <span slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">取 消</el-button>
      <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
    </span>
  </el-dialog>
</template>


<script>
export default {
  data() {
    return {
      type: 1,
      dialogVisible: false,
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
    };
  },
  methods: {
    open() {
      this.dialogVisible = true;
    },
  },
};
</script>

<style>
</style>