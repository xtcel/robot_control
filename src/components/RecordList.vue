// 记录列表

<template>
  <div class="record-list">
    <el-table :data="records" style="width: 100%" @cell-click="onCellClick" border stripe>
      <el-table-column prop="index" label="Index" width="180" />
      <el-table-column prop="name" label="Name" width="180" />
    </el-table>
    <!-- <div class="record-item" v-for="(record, index) in records" :key="index">
      <div class="name">{{ record }}</div>
    </div> -->
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
    }
  }
}
</script>

<style scoped></style>