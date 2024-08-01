// 记录列表

<template>
  <div class="record-list">
    <el-table :data="records" style="width: 100%" @cell-click="onCellClick" @row-mouse-enter="handleMouseEnter"
      @row-mouse-leave="handleMouseLeave" border stripe>
      <el-table-column prop="index" label="Index" width="180" />
      <el-table-column prop="name" label="Name" width="180">
        <template #default="{ row }">
          <el-button link type="primary" size="small">{{ row.name }}</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>

import { httpGet } from '@/utils/http';

export default {
  name: 'RecordList',
  data() {
    return {
      records: []
    }
  },
  mounted() {
    this.loadActions();
  },
  methods: {
    loadActions() {
      httpGet('/api/index.php', {
        action: 'list',
      }).then(response => {
        console.log(response);
        // const { code, message, data } = response;
        const datas = response.data;
        // to object list
        this.records = Object.keys(datas).map(key => {
          return {
            index: key,
            name: datas[key]
          }
        });
        console.log(this.records);
      }).catch(error => {
        console.error(error);
      })
    },
    onCellClick(row, column, cell, event) {
      console.log(row, column, cell, event);
      //  /index.php?action=show&scene=happy_60
      httpGet('/api/index.php', {
        action: 'show',
        scene: row.name
      }).then(response => {
        console.log(response);

      }).catch(error => {
        console.error(error);
      })
    },
    handleMouseEnter(row, column, event) {
      console.log(row, column, event);
      event.currentTarget.style.backgroundColor = '#f0f0f0'; // 设置鼠标悬停时的背景色
    },
    handleMouseLeave(row, column, event) {
      console.log(row, column, event);

      event.currentTarget.style.backgroundColor = ''; // 恢复原背景色
    }
  }
}
</script>

<style scoped>
.el-table .el-table__row:hover {
  color: blueviolet;
}
</style>