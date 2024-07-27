<template>
  <div>
    <el-table :data="actions" style="width: 700px" @row-drag-end="onDragEnd" row-key="index" v-loading="loading"
      :sortable="true" stripe border>
      <el-table-column prop="name" edit label="序号/名称" width="80px"></el-table-column>
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
    <el-button type="primary" @click="executeAll">预览编排</el-button>
    <el-button type="primary" @click="executeAll">保存脚本</el-button>
  </div>
</template>

<script>
export default {
  props: ['actions', 'loading'],
  methods: {
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
