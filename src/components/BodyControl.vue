<template>
  <div>
    <h2>身体控制</h2>
    <label>左右旋转({{ bodyAngle }}°)</label>
    <input type="range" min="0" max="120" v-model="bodyAngle" @input="rotateBody(bodyAngle)"><button
      @click="onReset">复位</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bodyAngle: 60
    };
  },
  methods: {
    onReset() {
      this.bodyAngle = 60
    },
    rotateBody(angle) {
      this.$emit('control', { part: 'body', angle });
    }
  }
}
</script>
