<template>
  <div class="college-container">
    <!-- 提示信息 -->
    <div class="tips">
      <div class="tip-item">
        <i class="el-icon-warning"></i>
        入驻高校前，必须维护您的招聘专业，且必需有高校开设的专业才能入驻。
        <a href="#" class="link">点击进入招聘专业维护</a>
      </div>
      <div class="tip-item">
        <i class="el-icon-warning"></i>
        入驻高校后，由入驻学校就业部门负责审核。
      </div>
    </div>

    <!-- 筛选区域 -->
    <div class="filter-bar">
      <el-input
        v-model="filterForm.collegeName"
        placeholder="请输入院校名称"
        class="filter-input"
      ></el-input>
      <el-select
        v-model="filterForm.city"
        placeholder="请选择所在城市"
        class="filter-select"
      >
        <el-option
          v-for="city in cityOptions"
          :key="city"
          :label="city"
          :value="city"
        ></el-option>
      </el-select>
      <el-select
        v-model="filterForm.type"
        placeholder="请选择院校类型"
        class="filter-select"
      >
        <el-option
          v-for="type in typeOptions"
          :key="type"
          :label="type"
          :value="type"
        ></el-option>
      </el-select>
      <el-button type="primary" @click="handleSearch">搜索</el-button>
      <el-button @click="handleReset">重置</el-button>
    </div>

    <!-- 院校列表 -->
    <el-table
      :data="tableData"
      border
      style="width: 100%"
      class="college-table"
    >
      <el-table-column
        label="院校名称"
        align="center"
        class-name="table-header"
      >
        <template slot-scope="scope">
          <div class="college-info">
            <img
              :src="scope.row.logo"
              alt="logo"
              class="college-logo"
            />
            <div class="college-text">
              <span class="college-name">{{ scope.row.name }}</span>
              <el-tag type="success" class="college-tag"
              >{{ scope.row.tag }}</el-tag
              >
              <span class="college-city">{{ scope.row.city }}</span>
            </div>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        label="联系人"
        prop="contact"
        align="center"
        class-name="table-header"
      ></el-table-column>
      <el-table-column
        label="联系电话"
        prop="phone"
        align="center"
        class-name="table-header"
      ></el-table-column>
      <el-table-column
        label="电子邮箱"
        prop="email"
        align="center"
        class-name="table-header"
      ></el-table-column>
      <el-table-column
        label="入驻状态"
        align="center"
        class-name="table-header"
      >
        <template slot-scope="scope">
          <el-tag
            :type="scope.row.status === '待审核' ? 'warning' : 'success'"
          >{{ scope.row.status }}</el-tag
          >
        </template>
      </el-table-column>
      <el-table-column
        label="操作"
        align="center"
        class-name="table-header"
      >
        <template slot-scope="scope">
          <el-button
            type="primary"
            @click="handleView(scope.row)"
            v-if="scope.row.status === '待审核'"
          >查看</el-button
          >
          <el-button
            type="primary"
            disabled
            v-else
          >已入驻</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <pagination v-show="pagination.total>0" :total="pagination.total" :page.sync="pagination.currentPage" :limit.sync="pagination.pageSize" />
  </div>
</template>

<script>
export default {
  name: "CollegeTable",
  data() {
    return {
      // 筛选表单数据
      filterForm: {
        collegeName: "",
        city: "",
        type: "",
      },
      // 城市下拉选项（可根据实际接口返回动态生成）
      cityOptions: ["请选择所在城市", "安徽省合肥市", "北京市", "上海市"],
      // 院校类型下拉选项（可根据实际接口返回动态生成）
      typeOptions: ["请选择院校类型", "本科公办院校", "本科民办院校"],
      // 表格数据（模拟，实际需从接口获取）
      tableData: [
        {
          logo: "https://via.placeholder.com/40",
          name: "安徽生源国家优师专项师范毕业生",
          tag: "本科公办院校",
          city: "安徽省合肥市",
          contact: "",
          phone: "",
          email: "",
          status: "待审核",
        },
        {
          logo: "https://via.placeholder.com/40",
          name: "安徽大学",
          tag: "本科公办院校",
          city: "安徽省合肥市",
          contact: "",
          phone: "",
          email: "",
          status: "已入驻",
        },
        {
          logo: "https://via.placeholder.com/40",
          name: "中国科学技术大学",
          tag: "本科公办院校",
          city: "安徽省合肥市",
          contact: "",
          phone: "",
          email: "",
          status: "已入驻",
        },
        {
          logo: "https://via.placeholder.com/40",
          name: "合肥工业大学",
          tag: "本科公办院校",
          city: "安徽省合肥市",
          contact: "韩新节 李同庆",
          phone: "0551-62905826",
          email: "hfgdjob@163.com",
          status: "待审核",
        },
      ],
      // 分页数据
      pagination: {
        currentPage: 1,
        pageSize: 10,
        total: 1
      }
    };
  },
  methods: {
    // 搜索按钮点击事件
    handleSearch() {
      // 这里可根据 filterForm 数据调用接口，请求筛选后的表格数据
      console.log("搜索条件：", this.filterForm);
      // 示例：假设接口返回新数据，替换 tableData
      // this.tableData = ...
    },
    // 重置按钮点击事件
    handleReset() {
      this.filterForm.collegeName = "";
      this.filterForm.city = "";
      this.filterForm.type = "";
      // 可调用接口获取初始表格数据
    },
    // 查看按钮点击事件
    handleView(row) {
      console.log("查看院校详情：", row);
      // 可跳转详情页，或弹出弹窗展示详情逻辑
    },
  },
};
</script>

<style scoped>
.college-container {
  padding: 20px;
}
.tips {
  margin-bottom: 10px;
}
.tip-item {
  padding: 8px 12px;
  background: #fff9e6;
  border: 1px solid #ffe58f;
  border-radius: 4px;
  margin-bottom: 5px;
  display: flex;
  align-items: center;
}
.tip-item i {
  color: #f5a623;
  margin-right: 6px;
}
.link {
  color: #409eff;
  text-decoration: underline;
  margin-left: 4px;
  cursor: pointer;
}
.filter-bar {
  display: flex;
  align-items: center;
  margin-bottom: 16px;
}
.filter-input {
  width: 200px;
  margin-right: 10px;
}
.filter-select {
  width: 200px;
  margin-right: 10px;
}
.college-table {
  background: #fff;
}
.table-header {
  background: #f8f9fa;
}
.college-info {
  display: flex;
  align-items: center;
}
.college-logo {
  width: 40px;
  height: 40px;
  margin-right: 10px;
  border-radius: 4px;
}
.college-text {
  display: flex;
  flex-direction: column;
}
.college-name {
  font-weight: bold;
  margin-bottom: 4px;
}
.college-tag {
  margin-bottom: 4px;
  font-size: 12px;
}
.college-city {
  font-size: 12px;
  color: #999;
}
</style>
