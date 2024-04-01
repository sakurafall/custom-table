<template>
  <div class="approval">
    <el-button style="color: #fff; background: #486295; margin-bottom: 15px;" size="small" @click="dialogVisible = true" >+审批提交</el-button>
    <el-table
      :data="tableData"
      border
      :row-class-name="onTableRowClassName"
      :header-cell-style="{background:'#f2f6fa',color:'#7b8597'}"
      style="width: 100%">
      <el-table-column
        fixed
        prop="approvalName"
        label="审批名"
        >
      </el-table-column>
      <el-table-column
        prop="state"
        label="审批状态"
        >
        <template slot-scope="scope">
          {{ scope.row.state ? '已审核' : '未审核' }}
        </template>
      </el-table-column>
      <el-table-column
        prop="applicant"
        label="申请人"
        >
      </el-table-column>
      <el-table-column
        prop="approvalTime"
        label="审批时间"
        >
      </el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="100">
        <template slot-scope="scope">
          <el-button v-if="scope.row.state" @click="handleViewClick(scope.row)" size="small">查看</el-button>
          <el-button v-else @click="handleApprovalClick(scope.row)" style="color: #fff; background: #486295" size="small">审核</el-button>
        </template>
      </el-table-column>
    </el-table>

    
    <!-- 对话框：查看详情 -->
    <el-dialog title="详情" :visible.sync="dialogApprovalVisible">
      <el-form :model="approvalDialogData">
        <el-form-item label="审批ID：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.approvalId }}</div>
        </el-form-item>
        <el-form-item label="审批名称：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.approvalName }}</div>
        </el-form-item>
        <el-form-item label="审批状态：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.state ? '已审核' : '未审核' }}</div>
        </el-form-item>
        <el-form-item label="申请人：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.applicant }}</div>
        </el-form-item>
        <el-form-item label="审批时间：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.approvalTime }}</div>
        </el-form-item>
        <el-form-item label="审批类型：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.approvalType }}</div>
        </el-form-item>
        <el-form-item label="审批内容：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.approveContent }}</div>
        </el-form-item>
        <el-form-item label="审批意见：" :label-width="formLabelWidth">
          <div v-if="approvalDialogData.state" class="detail">{{ approvalDialogData.approvalOpinions }}</div>
          <el-input v-else type="textarea" v-model="approvalDialogData.approvalOpinions" placeholder="请输入意见"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogApprovalVisible = false" size="small">取 消</el-button>
        <el-button v-if="approvalDialogData.state" style="color: #fff; background: #486295" size="small" @click="dialogApprovalVisible = false">确 定</el-button>
        <el-button v-else style="color: #fff; background: #486295" size="small" @click="sureApproval(approvalDialogData)">审核</el-button>
      </div>
    </el-dialog>

    <!-- 对话框：查看详情 -->
    <el-dialog title="审批申请" :visible.sync="dialogVisible">
      <el-form class="form" :model="approvalDialogData">
        <el-form-item label="审批名称：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.approvalName }}</div>
        </el-form-item>
        <el-form-item label="申请人：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.applicant }}</div>
        </el-form-item>
        <el-form-item label="审批时间：" :label-width="formLabelWidth">
          <div class="detail">{{ approvalDialogData.approvalTime }}</div>
        </el-form-item>
        <el-form-item label="审批类型：" :label-width="formLabelWidth">
          <el-select v-model="optionsValue" placeholder="请选择">
            <el-option
              v-for="item in approvalTypeOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="审批内容：" :label-width="formLabelWidth">
          <el-input type="textarea" v-model="approvalDialogData.approvalOpinions" placeholder="请输入审批内容"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button style="color: #fff; background: #486295" size="small" @click="dialogVisible = false">提交</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        // 审批表格总数据
        tableData: 
        [
          {
            // id
            approvalId: 1,
            // 名称
            approvalName: 'aaaaaa',
            // 审批状体
            state: 1,
            // 申请人
            applicant: '张三',
            // 审批时间
            approvalTime: '2024-03-24',
            // 审批类型
            approvalType: '审批类型1',
            // 审批内容
            approveContent: '审批内容11',
            // 审批意见
            approvalOpinions: '审批意见1111'
          },
          {
            approvalId: 2,
            approvalName: 'bbbbbb', 
            state: 1,
            applicant: '李四',
            approvalTime: "2024-03-25",
            approvalType: '审批类型2',
            approveContent: '审批内容22',
            approvalOpinions: '审批意见2222'
          },
          {
            approvalId: 3,
            approvalName: 'cccccc',
            state: 0,
            applicant: '王五',
            approvalTime: "2024-03-26",
            approvalType: '审批类型3',
            approveContent: '审批内容33',
            approvalOpinions: '审批意见3333'
          }, 
          {
            approvalId: 4,
            approvalName: 'dddddd',
            state: 0,
            applicant: '赵六',
            approvalTime: "2024-03-24",
            approvalType: '审批类型4',
            approveContent: '审批内容44',
            approvalOpinions: '审批意见4444'
          },
        ],

        dialogApprovalVisible: false,
        dialogVisible: true,
        // 详情数据
        approvalDialogData: {
          approvalId: undefined,
          approvalName: '加班申请',
          state: '',
          applicant: '王鹤松',
          approvalTime: '2024-03-25',
          approvalType: '',
          approveContent: '',
          approvalOpinions: ''
        },

        approvalTypeOptions: [{
            value: '选项1',
            label: '加班申请'
          }, {
            value: '选项2',
            label: '请假申请'
          }, {
            value: '选项3',
            label: '出差申请'
          }],
        optionsValue: '',

        formLabelWidth: '120px'
      }
    },
    methods: {
      // 操作列 查看按钮事件
      handleViewClick(row) {
        this.approvalDialogData = row
        this.dialogApprovalVisible = true
      },
      // 操作列 审批按钮事件
      handleApprovalClick(row) {
        this.approvalDialogData = row
        this.dialogApprovalVisible = true
      },
      // 详情 审批按钮事件
      sureApproval(data) {
        data.state = 1
        this.tableData.forEach(el => {
          if (el.approvalId === data.approvalId) {
            el.state = 1
            el.approvalOpinions = data.approvalOpinions
          }
        })
        this.dialogApprovalVisible = false
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
  }
</script>

<style>
/* 整体页面边距 */
.approval {
  margin: 20px;
}

.form {
  border: 1px  #999;
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