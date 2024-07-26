<template>
  <div>
    <h2>手臂控制</h2>
    <label>左臂({{ Math.abs(leftArmAngle - 140) }}°)</label>
    <input type="range" min="0" max="140" v-model="leftArmAngle" @input="updateArm('left', leftArmAngle)">
    <label>右臂({{ rightArmAngle }}°)</label>
    <input type="range" min="0" max="140" v-model="rightArmAngle" @input="updateArm('right', rightArmAngle)">
    <button @click="onReset">复位</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // min 140 , max 0 (需要-140 取反)
      leftArmAngle: 0,
      // min 0 max 140
      rightArmAngle: 0
    };
  },
  methods: {
    onReset() {
      this.leftArmAngle = 0;
      this.rightArmAngle = 0;
    },
    updateArm(side, angle) {
      this.$emit('control', { part: 'arm', side, angle });
    }
  }
}
</script>
