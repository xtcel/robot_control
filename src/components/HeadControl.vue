<template>
  <div>
    <h2>头部控制</h2>
    <label>抬头低头({{ upDownAngle }}°)</label>
    <input type="range" min="0" max="80" v-model="upDownAngle" @input="updateHead('upDown', upDownAngle)">
    <label>左右旋转({{ leftRightAngle }}°)</label>
    <input type="range" min="0" max="120" v-model="leftRightAngle" @input="updateHead('leftRight', leftRightAngle)">
    <button @click="onReset">复位</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      upDownAngle: 40,
      leftRightAngle: 60
    };
  },
  methods: {
    onReset() {
      this.upDownAngle = 40;
      this.leftRightAngle = 60;
    },
    updateHead(direction, angle) {

      // this.$emit('control', { part: 'head', direction, angle });
    }
  }
}
</script>
