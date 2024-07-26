<template>
  <div>
    <el-form label-position="top" label-width="100px">
      <el-form-item label="头部控制">
        <el-slider v-model="headUpDown" :min="0" :max="80" :step="1" @change="updateCommandPreview"></el-slider>
        <el-slider v-model="headLeftRight" :min="0" :max="120" :step="1" @change="updateCommandPreview"></el-slider>
      </el-form-item>

      <el-form-item label="手臂控制">
        <el-slider v-model="leftArm" :min="0" :max="140" :step="1" reverse @change="updateCommandPreview"></el-slider>
        <el-slider v-model="rightArm" :min="0" :max="140" :step="1" @change="updateCommandPreview"></el-slider>
      </el-form-item>

      <el-form-item label="身体控制">
        <el-slider v-model="bodyRotation" :min="0" :max="120" :step="1" @change="updateCommandPreview"></el-slider>
      </el-form-item>

      <el-form-item label="动作速度">
        <el-radio-group v-model="speed" @change="updateCommandPreview">
          <el-radio :label="1">慢 1</el-radio>
          <el-radio :label="3">中 3</el-radio>
          <el-radio :label="6">快 6</el-radio>
          <el-radio :label="8">较快 8</el-radio>
          <el-radio :label="10">很快 10</el-radio>
        </el-radio-group>
      </el-form-item>

      <el-form-item label="动作时延">
        <el-input-number v-model="interval" :min="0.02" :max="0.1" :step="0.02"
          @change="updateCommandPreview"></el-input-number>
      </el-form-item>

      <el-form-item label="指令预览">
        <el-input type="textarea" :value="commandPreview" readonly></el-input>
      </el-form-item>

      <el-button type="primary" @click="addAction">加入动作</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      headUpDown: 40,
      headLeftRight: 60,
      leftArm: 140,
      rightArm: 0,
      bodyRotation: 60,
      speed: 1,
      interval: 0.02,
      commandPreview: ''
    };
  },
  methods: {
    getSpeedString(speed) {
      var speedString;
      if (speed == 1) {
        speedString = "slow";
      } else if (speed == 3) {
        speedString = "medium";
      } else if (speed == 6) {
        speedString = "fast";
      } else if (speed == 8) {
        speedString = "faster";
      } else if (speed == 10) {
        speedString = "fastest";
      }

      return speedString;
    },
    updateCommandPreview() {
      this.commandPreview = JSON.stringify({
        servo_0: this.headUpDown,
        servo_1: this.headLeftRight,
        servo_2: this.leftArm,
        servo_3: this.rightArm,
        servo_4: this.bodyRotation,
        speed: this.getSpeedString(this.speed),
        interval: this.interval
      });
    },
    addAction() {
      this.$emit('add-action', JSON.parse(this.commandPreview));
    }
  },
  mounted() {
    this.updateCommandPreview();
  }
};
</script>
