<template>
  <div>
    <el-table :data="actions" style="width: 100%" @row-drag-end="onDragEnd" row-key="index" v-loading="loading"
      :sortable="true" border>
      <el-table-column type="index" label="Index" width="80"></el-table-column>
      <el-table-column prop="command" label="动作组" min-width="180"></el-table-column>
      <el-table-column prop="speed" label="舵机速度"></el-table-column>
      <el-table-column prop="interval" label="时延"></el-table-column>
      <el-table-column label="操作" width="120">
        <template #default="scope">
          <el-button type="text" size="small" @click="executeAction(scope.$index)">执行</el-button>
          <el-button type="text" size="small" @click="removeAction(scope.$index)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div style="height: 20px;" />
    <el-button type="primary" @click="executeAll">执行编排动作</el-button>
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
    }
  }
};
</script>
