<template>
  <div class="task">
    <TableC :tableColumns="tableColumns" :tableDataC="tableDataC" :headerStyle="tableCustomize.headerStyle">  
      <template v-slot:address="{ row }">  
        {{ row.province }} {{ row.city }} {{ row.street }}
      </template> 
      <template v-slot:operation="{ row }">  
        <el-button @click="add" size="mini">+添加</el-button>
      </template> 
      <!-- 如果还有其他需要自定义渲染的列，可以在这里添加更多的 <template v-slot:...> -->  
    </TableC>  

  </div>
</template>

<script>
  import TableC from '@/components/common/tableC.vue'
  export default {
    components: {
      TableC
    },
    data() {
      return {
        tableCustomize: {
          headerStyle: { background: '#fafafa', color: '#409eff', padding: '10' },
        },
        // 任务表格总数据
        tableColumns:  [  
          { id: 0, prop: 'date', label: '日期', width: 180, show: true},  
          { id: 1, prop: 'name', label: '姓名', width: 180, show: true},  
          {  
            id: 2,
            prop: 'address',  
            label: '地址',   
            width: 300,
            show: true,
            renderName: 'address',
            renderProps: (row, index) => ({ row, index })
          },
          {  
            id: 3,
            prop: 'tag',  
            label: '标签',   
            width: 300,
            show: false,
            renderName: 'tag',
            renderProps: (row, index) => ({ row, index })
          },
          {
            id: 4,
            prop: 'operation',
            label: '操作',
            width: 400,
            show: true,
            renderName: 'operation',
            renderProps: (row, index) => ({ row, index })
          },
        ],
        tableDataC: [  
          { date: '2023-04-01', name: '张三', province: '广东省', city: '广州市', street: '天河路' },  
          { date: '2023-04-02', name: '李四', province: '北京市', city: '朝阳区', street: '三里屯' },  
          // 更多行...  
        ]
      }
    },
    methods: {
      add() {
        console.log('111');
      }
    }
  }
</script>

<style>
.task {
  margin: 20px
}
</style>