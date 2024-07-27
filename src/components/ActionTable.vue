<template>
  <div>
    <el-table :data="actions" style="width: 700px" @row-drag-end="onDragEnd" row-key="index" v-loading="loading"
      :sortable="true" @cell-click="showUnitInput" stripe border >
      <el-table-column prop="name" edit label="序号/名称" width="80px">
        <template #default="{ row, column, $index}">
          <el-input
            v-if="
            tableRowEditId === $index  &&
            tableColumnEditIndex === column.id
            "
            v-model="row.name"
            @input="onInputChange"
            @blur="blurValueInput(row, column)"
            @keyup.enter="blurValueInput(row, column)"
          />
          <span v-else>{{ row.name }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="command" label="动作组" :formatter="fmtCommand" min-width="180px"></el-table-column>
      <el-table-column prop="speed" label="舵机速度"></el-table-column>
      <el-table-column prop="interval" label="时延"></el-table-column>
      <el-table-column label="操作" width="150px">
        <template #default="scope">
          <el-button type="text" size="small" @click="executeAction(scope.$index)">执行</el-button>
          <el-button type="text" size="small" @click="removeAction(scope.$index)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div style="height: 20px;" />
    <el-button type="primary" @click="onReset">舵机复位</el-button>
    <el-button type="primary" @click="executeAll">预览编排</el-button>
    <el-button type="primary" @click="saveDatas">保存脚本</el-button>
  </div>
</template>

<script>
export default {
  props: ['actions', 'loading'],
  data() {
    return {
      tableRowEditId: null,
      tableColumnEditIndex: null,
      listConfig: [
        {
          mode: "text",
          label: "姓名",
          prop: "name",
          minWidth: "100px",
          disabled(row) {
            return row.name === "cherry" ? true : false;
          },
          change: (v, row, index) => {
            console.log("v, row, index: ", v, row, index);
          },
          blur: (v) => {
            console.log("name blur ", v);
          },
          rules: [
            {
              type: "string",
              required: true,
              message: "姓名不能为空",
            },
          ],
        },
      ],

    };
  },
  methods: {
    onInputChange(value) {
      console.log("onInputChange", value);
      this.$emit('update-action-name', { index: this.tableRowEditId, name: value });
    },
    executeAction(index) {
      this.$emit('execute-action', index);
    },
    removeAction(index) {
      this.$emit('remove-action', index);
    },
    executeAll() {
      this.$emit('execute-all');
    },
    onDragEnd(newIndex, oldIndex) {
      this.$emit('drag-end', { newIndex, oldIndex });
    },
    saveDatas() {
      this.$emit('save-datas');
    },
    showUnitInput(row, column, cell, event ) {
      console.log('onClick : row', row)
      console.log("cell", cell);
      console.log("column", column);
      console.log("event", event);
      //赋值给定义的变量
      const index = this.actions.indexOf(row);
      console.log("index", index);
      this.tableRowEditId = index; //确定点击的单元格在哪行 如果数据中有ID可以用ID判断，没有可以使用其他值判断，只要能确定是哪一行即可
      this.tableColumnEditIndex = column.id; //确定点击的单元格在哪列 
    },
    blurUnitInput(row, column) {
      console.log('blurValueInput : row', row)
      console.log("column", column);
      this.tableRowEditId = null
      this.tableColumnEditIndex = null
      //在此处调接口传数据
    },
    blurValueInput(row, column) {
      console.log('blurValueInput : row', row)
      console.log("column", column);
      this.tableRowEditId = null
      this.tableColumnEditIndex = null
      //在此处调接口传数据
    },
    fmtCommand(row, column, cellValue, index) {
      console.log(index);
      console.log(row);
      const { servo0, servo1, servo2, servo3, servo4 } = row;
      var result = '';
      if (typeof servo0 != 'undefined') {
        result += `舵机 0: ${servo0}° ,`;
      }
      if (typeof servo1 != 'undefined') {
        result += `舵机 1: ${servo1}° ,`;
      }
      if (typeof servo2 != 'undefined') {
        result += `舵机 2: ${servo2}° ,`;
      }
      if (typeof servo3 != 'undefined') {
        result += `舵机 3: ${servo3}° ,`;
      }
      if (typeof servo4 != 'undefined') {
        result += `舵机 4: ${servo4}° ,`;
      }
      if (result === '') {
        result = '无动作';
      }
      console.log(result);
      return result;
    }
  }
};
</script>
