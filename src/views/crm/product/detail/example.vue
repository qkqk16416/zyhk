<template>
  <el-table :data="tableData" row-key="id">
    <el-table-column prop="name" label="名称(A列)"></el-table-column>
    <el-table-column type="expand">
      <template slot-scope="props">
        <el-table :data="props.row.wafs" class="inner-table">
          <el-table-column prop="wafName" label="WAF属性(B列)"></el-table-column>
          <el-table-column type="expand">
            <template slot-scope="innerProps">
              <el-table :data="innerProps.row.properties" class="inner-most-table">
                <el-table-column prop="propName" label="C列属性"></el-table-column>
              </el-table>
            </template>
          </el-table-column>
        </el-table>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
export default {
  name: 'NestedTable',
  data() {
    return {
      tableData: [
        {
          id: 1,
          name: '名称1',
          wafs: [
            {
              id: 11,
              wafName: 'WAF1',
              properties: [
                { id: 111, propName: '属性1' },
                { id: 112, propName: '属性2' }
              ]
            },
            {
              id: 12,
              wafName: 'WAF2',
              properties: [
                { id: 121, propName: '属性3' },
                { id: 122, propName: '属性4' }
              ]
            }
          ]
        },
        {
          id: 2,
          name: '名称2',
          wafs: [
            {
              id: 21,
              wafName: 'WAF3',
              properties: [
                { id: 211, propName: '属性5' },
                { id: 212, propName: '属性6' }
              ]
            }
          ]
        }
      ]
    }
  }
}
</script>

<style scoped>
.inner-table {
  margin-left: 50px;
}
.inner-most-table {
  margin-left: 50px;
}
</style>