<template>
  <div id="app">
    <div className="container">
      <el-card >
        <control-panel @add-action="addAction" />
      </el-card>
      <div style="width: 10px;" />
      <el-card >
      <action-table :actions="actions" :loading="loading" @execute-action="executeAction" @remove-action="removeAction"
        @execute-all="executeAll" @drag-end="onDragEnd" />
    </el-card>
    </div>
  </div>
</template>

<script>
import ControlPanel from './components/ControlPanel.vue';
import ActionTable from './components/ActionTable.vue';

export default {
  components: {
    ControlPanel,
    ActionTable
  },
  data() {
    return {
      actions: [],
      loading: false
    };
  },
  methods: {
    addAction(action) {
      this.actions.push(action);
    },
    executeAction(index) {
      const action = this.actions[index];
      this.sendCommand(action);
    },
    removeAction(index) {
      this.actions.splice(index, 1);
    },
    executeAll() {
      this.loading = true;
      let index = 0;
      const executeNext = () => {
        if (index < this.actions.length) {
          this.sendCommand(this.actions[index]).then(() => {
            index++;
            executeNext();
          });
        } else {
          this.loading = false;
        }
      };
      executeNext();
    },
    sendCommand(command) {
      return new Promise((resolve) => {
        setTimeout(() => {
          console.log('发送命令:', command);
          resolve();
        }, 1000);
      });
    },
    onDragEnd({ newIndex, oldIndex }) {
      const movedItem = this.actions.splice(oldIndex, 1)[0];
      this.actions.splice(newIndex, 0, movedItem);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  text-align: center;
}

.container {
  display: flex;
  flex-direction: row;
  align-content: space-between;
  margin: 1rem 1rem;
}

.el-form-item {
  margin-bottom: 20px;
}
</style>


<!-- <template>
  <div id="app">
    <h1>机器人控制编排页面</h1>
    <HeadControl @control="recordAction" />
    <ArmControl @control="recordAction" />
    <BodyControl @control="recordAction" />

    <h2>操作记录</h2>
    <table>
      <thead>
        <tr>
          <th>部位</th>
          <th>方向</th>
          <th>角度</th>
          <th>时间</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(action, index) in actions" :key="index">
          <td>{{ action.part }}</td>
          <td>{{ action.direction }}</td>
          <td>{{ action.angle }}°</td>
          <td>{{ action.time }}</td>
        </tr>
      </tbody>
    </table>

    <button @click="saveActions">保存编排动作</button>
  </div>
</template>

<script>
import HeadControl from './components/HeadControl.vue'
import ArmControl from './components/ArmControl.vue'
import BodyControl from './components/BodyControl.vue'

export default {
  components: {
    HeadControl,
    ArmControl,
    BodyControl
  },
  data() {
    return {
      actions: []
    };
  },
  methods: {
    recordAction(action) {
      action.time = new Date().toLocaleString();
      this.actions.push(action);
    },
    saveActions() {
      const blob = new Blob([JSON.stringify(this.actions, null, 2)], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const link = document.createElement('a');
      link.href = url;
      link.download = 'actions.json';
      link.click();
      URL.revokeObjectURL(url);
    }
  }
}
</script> -->

<!-- <style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  text-align: center;
}

table {
  margin: 20px auto;
  border-collapse: collapse;
  width: 80%;
}

th,
td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: center;
}

button {
  margin: 10px;
}
</style> -->