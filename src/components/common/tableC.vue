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
      <el-table-column align="right" fixed="right" :key="Math.random()">
        <!-- 表格筛选 -->
        <!-- <template slot="header">
          <el-button type="text" icon="el-icon-setting"></el-button>
        </template> -->
        <template slot-scope="scope">
          <el-button
            size="mini"
            type="text"
            @click="() => console.log(scope)">详情</el-button>
        </template>
      </el-table-column>
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
          <el-table-column align="right" :key="Math.random()">
            <!-- 表格筛选 -->
            <!-- <template slot="header">
              <el-button type="text" icon="el-icon-setting"></el-button>
            </template> -->
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
              class="input-width"
              placeholder="请输入内容"
              v-model="tableProps.label"
              size="small"
              clearable>
            </el-input>
          </div>
          <div class="settings-item">
            <span class="name">是否显示：</span>
            <el-radio-group v-model="tableProps.show" size="small">
              <el-radio-button label="显示"></el-radio-button>
              <el-radio-button label="隐藏"></el-radio-button>
            </el-radio-group>
          </div>
          <div class="settings-item">
            <span class="name">表列宽度：</span>
            <el-input
              v-model="tableProps.width"
              class="input-width"
              type="number" 
              size="small">
            </el-input>
          </div>
        </div>
      </div>
    </el-dialog>
  </div>  
</template>  
  
<script>  
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
      tableProps: {
        prop: '',  
        label: '',   
        width: undefined,
        show: true,
      },
      dialogSetTableVisible: false,
      
    }
  },
  methods: {
    setTable(column, event) {
      console.log(column, event)
      this.tableColumns.forEach(el => {
        if (el.label === column.label) {
          this.tableProps.prop = el.prop
          this.tableProps.label = el.label
          this.tableProps.width = el.width
          this.tableProps.show = el.show
        }
      })
      this.$emit('setTable', column, event)
    }
  }
};  
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
</style>