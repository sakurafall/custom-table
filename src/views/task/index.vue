<template>
  <div class="task">
    <el-card class="box-card analyse-card">
      <div class="analyse">
        <div class="left">
          <el-progress type="dashboard" :percentage="taskProcess" :color="colors"></el-progress>
          <div class="process-name">任务进度</div>
        </div>
        <div class="right">
          <div class="title">任务总计</div>
          <div class="task-detail">
            <div class="total">
              <div class="name">总计</div>
              <div class="num">{{ taskTotal }}</div>
            </div>
            <div class="finish">
              <div class="name">已完成</div>
              <div class="num">{{ taskFinish }}</div>
            </div>
            <div class="postpone">
              <div class="name">延期</div>
              <div class="num">{{ taskPostpone }}</div>
            </div>
          </div>
        </div>
      </div>
    </el-card>

    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span class="process-name">任务列表</span>
      </div>
      <div>
        <el-button style="color: #fff; background: #486295; margin-bottom: 15px;" size="small" @click="dialogTaskVisible = true" >+添加任务</el-button>
        <el-table
          :data="tableData"
          border
          :row-class-name="onTableRowClassName"
          :header-cell-style="{background:'#f2f6fa',color:'#7b8597'}"
          style="width: 100%">
          <el-table-column
            fixed
            prop="taskName"
            label="任务名称"
            >
          </el-table-column>
          <el-table-column
            prop="deadline"
            label="截止日期"
            >
          </el-table-column>
          <el-table-column
            prop="state"
            label="状态"
            >
            <template slot-scope="scope">
              <el-button v-if="scope.row.state" type="success" size="small">完成</el-button>
              <el-button v-else type="danger" size="small">延期</el-button>
            </template>
          </el-table-column>
          <el-table-column
            prop="applicant"
            label="发布人"
            >
          </el-table-column>
          
          <el-table-column
            fixed="right"
            label="操作"
            width="100">
            <template slot-scope="scope">
              <el-button @click="handleViewClick(scope.row)" size="small">查看</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-card>


    <!-- 对话框：查看详情 -->
    <el-dialog title="详情" :visible.sync="dialogTaskVisible">
      <el-form :model="taskDialogData">
        <el-form-item label="任务名称：" :label-width="formLabelWidth">
          <div class="detail">{{ taskDialogData.taskName }}</div>
        </el-form-item>
        <el-form-item label="任务描述：" :label-width="formLabelWidth">
          <div class="detail">{{ taskDialogData.taskDesc }}</div>
        </el-form-item>
        <el-form-item label="任务状态：" :label-width="formLabelWidth">
          <el-button v-if="taskDialogData.state" type="success" size="small">完成</el-button>
          <el-button v-else type="danger" size="small">延期</el-button>
        </el-form-item>
        <el-form-item label="截止时间：" :label-width="formLabelWidth">
          <div class="detail">{{ taskDialogData.deadline }}</div>
        </el-form-item>
        <el-form-item label="发送人" :label-width="formLabelWidth">
          <div class="detail">{{ taskDialogData.applicant }}</div>
        </el-form-item>
        <el-form-item label="接受人" :label-width="formLabelWidth">
          <div class="detail">{{ taskDialogData.recipients }}</div>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogTaskVisible = false" size="small">取 消</el-button>
        <el-button v-if="taskDialogData.state" style="color: #fff; background: #486295" size="small" @click="dialogTaskVisible = false">确 定</el-button>
      </div>
    </el-dialog>

    <!-- 对话框：添加任务 -->
    <el-dialog title="添加任务" :visible.sync="dialogTaskVisible">
      <el-form class="form" :model="taskDialogData">
        <el-form-item label="任务名称：" :label-width="formLabelWidth">
          <div class="detail">{{ taskDialogData.taskName }}</div>
        </el-form-item>
        <el-form-item label="申请人：" :label-width="formLabelWidth">
          <div class="detail">{{ taskDialogData.applicant }}</div>
        </el-form-item>
        <el-form-item label="任务时间：" :label-width="formLabelWidth">
          <div class="detail">{{ taskDialogData.deadline }}</div>
        </el-form-item>
        <el-form-item label="任务内容：" :label-width="formLabelWidth">
          <el-input type="textarea" v-model="taskDialogData.taskDesc" placeholder="请输入任务内容"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button style="color: #fff; background: #486295" size="small" @click="dialogTaskVisible = false">提交</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        // 任务表格总数据
        tableData:
        [
          {
            // id
            taskId: 1,
            // 名称
            taskName: 'aaaaaa',
            // 任务描述
            taskDesc: '任务描述1',
            // 任务状态
            state: 1,
            // 申请人
            applicant: '张三',
            // 接收人
            recipients: '接收人',
            // 任务时间
            deadline: '2024-03-24'
          },
          {
            taskId: 2,
            // 名称
            taskName: 'bbbbbb',
            // 任务描述
            taskDesc: '任务描述2',
            // 任务状态
            state: 0,
            // 申请人
            applicant: '张三',
            // 接收人
            recipients: '接收人',
            // 任务时间
            deadline: '2024-03-24'
          },
          {
            taskId: 3,
            // 名称
            taskName: 'cccccc',
            // 任务描述
            taskDesc: '任务描述3',
            // 任务状态
            state: 1,
            // 申请人
            applicant: '张三',
            // 接收人
            recipients: '接收人',
            // 任务时间
            deadline: '2024-03-24'
          },
          {
            taskId: 4,
            // 名称
            taskName: 'dddddd',
            // 任务描述
            taskDesc: '任务描述4',
            // 任务状态
            state: 1,
            // 申请人
            applicant: '张三',
            // 接收人
            recipients: '接收人',
            // 任务时间
            deadline: '2024-03-24'
          },
        ],
        // 控制对话框显隐
        dialogTaskVisible: false,
        // 详情数据：默认值就是添加任务的默认值
        taskDialogData: {
            taskId: undefined,
            taskName: 'aaaaaa',
            taskDesc: '',
            state: 1,
            applicant: '',
            recipients: '',
            deadline: '2024-03-24'
        },
        formLabelWidth: '120px',

        // 进度条
        percentage: 80,
        colors: [
          {color: '#f56c6c', percentage: 20},
          {color: '#e6a23c', percentage: 40},
          {color: '#5cb87a', percentage: 60},
          {color: '#1989fa', percentage: 80},
          {color: '#6f7ad3', percentage: 100}
        ]
      }
    },
    methods: {
      // 操作列 查看按钮事件
      handleViewClick(row) {
        this.dialogTaskVisible = true
        this.taskDialogData = row
      },
      // 斑马纹颜色
      onTableRowClassName({row, rowIndex}) {
          if (rowIndex%2!==0) {
          return 'statistics-warning-row';
        } else {
          return '';
        }
      }
    },
    computed: {
      taskTotal() {
        return this.tableData.length
      },
      taskFinish() {
        let finishData = this.tableData.filter(el => el.state === 1)
        return finishData.length
      },
      taskPostpone() {
        let postponeData = this.tableData.filter(el => el.state === 0)
        return postponeData.length
      },
      taskProcess() {
        let total = this.tableData.length;  
        let finishNum = this.tableData.filter(el => el.state === 1).length
          
        // 计算完成的百分比  
        let completionPercentage = (finishNum / total) * 100
          
        // 如果需要，您可以四舍五入到小数点后两位  
        completionPercentage = completionPercentage.toFixed(2)
        console.log(completionPercentage);
          
        return Number(completionPercentage);

      }
    }
  }
</script>

<style>
/* 整体页面边距 */
.task {
  margin: 20px;
}

.analyse-card {
  margin-bottom: 20px;
}

.analyse {
  display: flex;
}

.analyse .left {
  width: fit-content;
  text-align: center;
  border-right: 1px solid #cac9c9;
  padding: 0 80px;
}

.process-name {
  font-size: 16px;
  font-weight: 600;
}

.analyse .right {
  width: 75%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.analyse .right .title {
  color: #344148;
  font-size: 16px;
  font-weight: 700;
  margin-left: 80px;
}

.analyse .right .task-detail {
  display: flex;
  justify-content: space-evenly;
  color: #344148;
  font-size: 16px;
  font-weight: 700;
  margin-left: 60px;
}

.analyse .right .task-detail .name {
  font-size: 15px;
  margin-bottom: 20px;
}

.analyse .right .task-detail .num {
  color: #486295;
  font-size: 24px;
  text-align: center;
}

/* 表头颜色 */
.el-table__row.statistics-warning-row {
  background: #f2f6fa;
}

.detail {
  font-size: 16px;
  background: #f2f6fa;
  padding-left: 10px;
  border-radius: 5px ;
}
</style>