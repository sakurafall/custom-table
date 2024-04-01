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
          :key="column.id"
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

    <el-dialog title="设置" :visible.sync="dialogSetTableVisible" :modal="true" width="60%">
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
        > 
          <template v-for="column in tableColumns">
            <el-table-column  
              v-if="column.show"
              :key="column.id"
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
        <!-- 设置项 -->
        <div class="settings">
          <div class="settings-item">
            <span></span>
            <el-input></el-input>
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
      dialogSetTableVisible: false,
      
    }
  },
  methods: {
  }
};  
</script>

<style lang="scss" scoped>
.dialog-set {
  display: flex;
  justify-content: space-evenly;
}
</style>