<template>
  <div class="resume-filter">
    <!-- 筛选条件区域 -->
    <el-form :inline="true" :model="filterForm" class="filter-form">
      <el-form-item label="院校名称">
        <el-select
          v-model="filterForm.collegeName"
          placeholder="请选择院校名称"
          clearable
        >
          <el-option
            v-for="item in collegeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="工作类型">
        <el-select
          v-model="filterForm.workType"
          placeholder="请选择工作类型"
          clearable
        >
          <el-option
            v-for="item in workTypeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="学历层次">
        <el-select
          v-model="filterForm.education"
          placeholder="请选择学历层次"
          clearable
        >
          <el-option
            v-for="item in educationOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="专业名称">
        <el-select
          v-model="filterForm.major"
          placeholder="请选择专业名称"
          clearable
        >
          <el-option
            v-for="item in majorOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="匹配模式">
        <el-select
          v-model="filterForm.matchMode"
          placeholder="请选择匹配模式"
          clearable
        >
          <el-option
            v-for="item in matchModeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="工作地点">
        <el-select
          v-model="filterForm.workLocation"
          placeholder="请选择工作地点"
          clearable
        >
          <el-option
            v-for="item in workLocationOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="职位类别">
        <el-select
          v-model="filterForm.jobCategory"
          placeholder="请选择职位类别"
          clearable
        >
          <el-option
            v-for="item in jobCategoryOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="薪资待遇">
        <el-select
          v-model="filterForm.salary"
          placeholder="请选择薪资待遇"
          clearable
        >
          <el-option
            v-for="item in salaryOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="任职经历">
        <el-select
          v-model="filterForm.workExperience"
          placeholder="请选择任职经历"
          clearable
        >
          <el-option
            v-for="item in workExperienceOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="更新时间">
        <el-date-picker
          v-model="filterForm.updateTime"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          clearable
        />
      </el-form-item>
      <el-form-item label="获奖经历">
        <el-select
          v-model="filterForm.awardExperience"
          placeholder="请选择获奖经历"
          clearable
        >
          <el-option
            v-for="item in awardExperienceOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="社团经历">
        <el-select
          v-model="filterForm.clubExperience"
          placeholder="请选择社团经历"
          clearable
        >
          <el-option
            v-for="item in clubExperienceOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="实习经历">
        <el-select
          v-model="filterForm.internship"
          placeholder="请选择实习经历"
          clearable
        >
          <el-option
            v-for="item in internshipOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="技能证书">
        <el-select
          v-model="filterForm.certificate"
          placeholder="请选择技能证书"
          clearable
        >
          <el-option
            v-for="item in certificateOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="handleSearch">搜索</el-button>
      </el-form-item>
      <el-form-item>
        <el-button @click="handleReset">重置</el-button>
      </el-form-item>
    </el-form>

    <!-- 简历列表表格 -->
    <el-table :data="tableData" border style="width: 100%; margin-top: 20px">
      <el-table-column
        prop="name"
        label="姓名"
        min-width="80"
      />
      <el-table-column
        prop="gender"
        label="性别"
        min-width="60"
      />
      <el-table-column
        prop="education"
        label="学历"
        min-width="80"
      />
      <el-table-column
        prop="collegeName"
        label="院校名称"
        min-width="120"
      />
      <el-table-column
        prop="major"
        label="专业名称"
        min-width="120"
      />
      <el-table-column
        prop="expectedCity"
        label="期望城市"
        min-width="100"
      />
      <el-table-column
        prop="expectedJobCategory"
        label="期望职位类别"
        min-width="120"
      />
      <el-table-column
        prop="expectedSalary"
        label="期望薪资"
        min-width="120"
      />
      <el-table-column
        prop="updateTime"
        label="更新时间"
        min-width="120"
      />
      <el-table-column
        label="操作"
        min-width="80"
      >
        <template slot-scope="scope">
          <el-button
            type="text"
            @click="handleDetail(scope.row)"
          >详情</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页组件 -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagination.currentPage"
      :page-sizes="[10, 20, 50]"
      :page-size="pagination.pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="pagination.total"
      style="margin-top: 20px; text-align: right"
    />
  </div>
</template>

<script>
export default {
  name: 'ResumeFilter',
  data() {
    return {
      // 筛选表单数据
      filterForm: {
        collegeName: '',
        workType: '',
        education: '',
        major: '',
        matchMode: '',
        workLocation: '',
        jobCategory: '',
        salary: '',
        workExperience: '',
        updateTime: [],
        awardExperience: '',
        clubExperience: '',
        internship: '',
        certificate: ''
      },
      // 下拉选项（模拟，实际需接口获取）
      collegeOptions: [
        { label: '安徽大学', value: '安徽大学' },
        { label: '合肥工业大学', value: '合肥工业大学' }
      ],
      workTypeOptions: [
        { label: '全职', value: '全职' },
        { label: '兼职', value: '兼职' }
      ],
      educationOptions: [
        { label: '本科', value: '本科' },
        { label: '硕士', value: '硕士' }
      ],
      majorOptions: [
        { label: '计算机科学与技术', value: '计算机科学与技术' },
        { label: '软件工程', value: '软件工程' }
      ],
      matchModeOptions: [
        { label: '精准匹配', value: '精准匹配' },
        { label: '模糊匹配', value: '模糊匹配' }
      ],
      workLocationOptions: [
        { label: '合肥', value: '合肥' },
        { label: '北京', value: '北京' }
      ],
      jobCategoryOptions: [
        { label: '技术/研发', value: '技术/研发' },
        { label: '产品', value: '产品' }
      ],
      salaryOptions: [
        { label: '8000-12000元/月', value: '8000-12000元/月' },
        { label: '15000-20000元/月', value: '15000-20000元/月' }
      ],
      workExperienceOptions: [
        { label: '有', value: '有' },
        { label: '无', value: '无' }
      ],
      awardExperienceOptions: [
        { label: '有', value: '有' },
        { label: '无', value: '无' }
      ],
      clubExperienceOptions: [
        { label: '有', value: '有' },
        { label: '无', value: '无' }
      ],
      internshipOptions: [
        { label: '有', value: '有' },
        { label: '无', value: '无' }
      ],
      certificateOptions: [
        { label: '有', value: '有' },
        { label: '无', value: '无' }
      ],
      // 表格数据（模拟）
      tableData: [
        {
          name: '张三',
          gender: '男',
          education: '本科',
          collegeName: '安徽大学',
          major: '计算机科学与技术',
          expectedCity: '合肥',
          expectedJobCategory: '技术/研发',
          expectedSalary: '8000-12000元/月',
          updateTime: '2025-04-15'
        },
        {
          name: '李四',
          gender: '男',
          education: '硕士',
          collegeName: '合肥工业大学',
          major: '软件工程',
          expectedCity: '北京',
          expectedJobCategory: '技术/研发',
          expectedSalary: '15000-20000元/月',
          updateTime: '2025-04-16'
        },
        // 其他模拟数据...
      ],
      // 分页配置
      pagination: {
        currentPage: 1,
        pageSize: 10,
        total: 8
      }
    }
  },
  methods: {
    // 搜索
    handleSearch() {
      console.log('筛选条件：', this.filterForm)
      // 调用接口逻辑：this.tableData = 接口返回数据; this.pagination.total = 接口返回总数
    },
    // 重置
    handleReset() {
      this.filterForm = {
        collegeName: '',
        workType: '',
        education: '',
        major: '',
        matchMode: '',
        workLocation: '',
        jobCategory: '',
        salary: '',
        workExperience: '',
        updateTime: [],
        awardExperience: '',
        clubExperience: '',
        internship: '',
        certificate: ''
      }
      this.handleSearch() // 重置后重新搜索
    },
    // 查看详情
    handleDetail(row) {
      console.log('简历详情：', row)
      this.$router.push({ path: '/resume/detail', query: { id: row.id } })
    },
    // 每页条数改变
    handleSizeChange(val) {
      this.pagination.pageSize = val
      this.handleSearch() // 切换条数后重新搜索
    },
    // 当前页改变
    handleCurrentChange(val) {
      this.pagination.currentPage = val
      this.handleSearch() // 切换页码后重新搜索
    }
  }
}
</script>

<style scoped>
.resume-filter {
  padding: 20px;
  background: #fff;
}
.filter-form {
  margin-bottom: 16px;
}
</style>
