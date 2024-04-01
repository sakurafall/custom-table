<!-- TableC.vue -->  
<template>  
  <div>
    <el-button type="text" icon="el-icon-setting" @click="dialogSetTableVisible = true">设置</el-button>
    <el-table 
      ref="tableTiem"
      :data="tableDataC"
      border
      size='mini'
      :header-cell-style="headerStyle"
      :row-style="{ padding: '0' }" 
      :cell-style="{ padding: '0' }"
    > 
      <template v-for="column in tableColumns">
        <el-table-column  
          v-if="column.show"
          :key="column.ids"
          :prop="column.prop"  
          :label="column.label"  
          :width="column.width"  
        >  
          <template  slot-scope="scope">  
            <!-- 使用作用域插槽传递数据 -->  
            <slot 
              v-if="column.renderName" :name="column.renderName" 
              v-bind="column.renderProps(scope.row, scope.$index)"
            ></slot>
            <!-- 如果没有自定义渲染，直接显示数据 --> 
            <span v-else>{{ scope.row[column.prop] }}</span>
          </template>  
        </el-table-column>
      </template>

      <!-- <el-table-column align="right" fixed="right" :key="Math.random()">
        <template slot="header">
          <el-button type="text" icon="el-icon-setting"></el-button>
        </template>
        <template slot-scope="scope">
          <el-button
            size="mini"
            type="text">详情</el-button>
        </template>
      </el-table-column> -->
    </el-table>  

    <el-dialog title="设置" :visible.sync="dialogSetTableVisible" :modal="true" width="90%">
      <div class="dialog-set">
        <!-- 表格 -->
        <el-table 
          ref="tableTiem"
          :data="tableDataC"
          border
          size='mini'
          :header-cell-style="headerStyle"
          :row-style="{ padding: '0' }" 
          :cell-style="{ padding: '0' }"
          @header-click="setTable"
          @header-dragend="dragendEvent"
        > 
          <template v-for="column in tableProps">
            <el-table-column  
              v-if="column.show"
              :key="column.ids"
              :prop="column.prop"  
              :label="column.label"  
              :width="column.width"  
            >  
              <template  slot-scope="scope">  
                <!-- 使用作用域插槽传递数据 -->  
                <slot 
                  v-if="column.renderName" :name="column.renderName" 
                  v-bind="column.renderProps(scope.row, scope.$index)"
                ></slot>
                <!-- 如果没有自定义渲染，直接显示数据 --> 
                <span v-else>{{ scope.row[column.prop] }}</span>
              </template>  
            </el-table-column>
          </template>
          <el-table-column align="right" :key="Math.random()">
            <template slot-scope="scope">
              <el-button
                size="mini"
                type="text"
                @click="() => console.log(scope)">详情</el-button>
            </template>
          </el-table-column>
        </el-table>

        <!-- 设置项 -->
        <div class="settings">
          <div class="settings-item">
            <span class="name">表头名称：</span>
            <el-input
              size="small"
              class="input-width"
              placeholder="请输入内容"
              v-model="tableProps[currentIndex].label"
              @input="headerNameChange"
              clearable>
            </el-input>
          </div>
          <div class="settings-item">
            <span class="name">表列宽度：</span>
            <el-input
              v-model="tableProps[currentIndex].width"
              class="input-width"
              type="number" 
              size="small">
            </el-input>
          </div>
          <div class="settings-item">
            <span class="name">是否显示：</span>
            <el-checkbox :indeterminate="isIndeterminate" v-model="checkAll" @change="handleCheckAllChange">全选</el-checkbox>
            <div style="margin: 15px 0;"></div>
              <el-checkbox-group v-model="checkedItem" @change="handleCheckedCitiesChange">
                <div class="show-checkbox">
                  <el-checkbox v-for="item in tableProps" :label="item.label" :key="item.ids">{{item.label}}</el-checkbox>
                </div>
              </el-checkbox-group>
          </div>
        </div>
      </div>
      <div class="footer">
        <el-button
          size="small"
          type="primary"
          @click="saveTable">保存</el-button>
        <el-button
          size="small"
          @click="dialogSetTableVisible = false">取消</el-button>
      </div>
    </el-dialog>
  </div>  
</template>  
  
<script>  
import _ from 'lodash'

export default {  
  props: {  
    tableColumns: {  
      type: Array,  
      default: () => []  
    },  
    tableDataC: {  
      type: Array,  
      default: () => []  
    },
    headerStyle: {
      type: Object,
      default: () => ({ background: '#fafafa', color: '#409eff', padding: '0' })
    }
  },   
  data() {
    return  {
      tableProps: [],
      currentIndex: 0,
      dialogSetTableVisible: false,
      showColumn: [],
      checkAll: false,
      checkedItem: [],
      headerList: [],
      isIndeterminate: false
    }
  },
  created() {
    this.tableProps = _.cloneDeep(this.tableColumns) // 深拷贝
    this.headerDataInit()
  },
  methods: {
    // 相关数据初始化操作
    headerDataInit() {
      this.headerList = this.tableProps.map(el => el.label)
      this.checkedItem = this.tableProps.map(el => {
        if (el.show === true) {
          return el.label
        }
      })
    },
    setTable(column, event) {
      this.currentIndex = this.tableProps.findIndex(el => {
        return el.prop == column.property
      })
    },
    // 拖拽边距操作
    dragendEvent(newWidth, oldWidth, column, event) {
      this.tableProps.forEach(el => {
        if (el.prop === column.property) {
          el.width = newWidth
        }
      })
    },
    // 全选操作
    handleCheckAllChange(val) {
      let tempList = this.tableProps.map(el => el.label)
      this.checkedItem = val ? tempList : [];
      this.isIndeterminate = false;
      tempList.forEach(el => {
        this.tableProps.forEach(item => {
          if (item.label === el) {
            item.show = val
          }
        })
      })
    },
    // 单选操作
    handleCheckedCitiesChange(value) {
      let checkedCount = value.length;
      this.checkAll = checkedCount === this.headerList.length;
      this.isIndeterminate = checkedCount > 0 && checkedCount < this.headerList.length
      this.tableProps.forEach(item => {
        if (value.includes(item.label)) {
          item.show = true
        } else {
          item.show = false
        }
      })
    },
    // 解决input改变name，headerList和checkedItem对应的name没有更新的问题
    headerNameChange(value) {
      this.headerDataInit()
    },
    saveTable() {
      this.$emit('saveTable', this.tableProps)
      this.dialogSetTableVisible = false
    }
  }
} 
</script>

<style lang="scss" scoped>
.dialog-set {
  display: flex;
  justify-content: space-evenly;

  .settings {
    width: 25%;
    margin-left: 20px;

    .settings-item {
      margin-bottom: 10px;
      .name {
        font-size: 14px;
        font-weight: 700;
      }
    }
  }
}

.input-width {  
  width: 150px;  
}

.show-checkbox {
  display: flex;
  flex-direction: column;
  margin-left: 70px;
}

.footer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
</style>