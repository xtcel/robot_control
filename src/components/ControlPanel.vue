<template>
  <div>
    <el-form label-position="top" label-width="auto" style="max-width: 500px;">
      <el-form-item label="头部控制">
        <div class="col">
          <div class="col">
            <div class="header-title">
              <el-text style="padding-right: 10px;">舵机 0</el-text><el-switch v-model="servo0On" @change="onChangeSwitch"></el-switch>
            </div>
            <div class="item">
              <el-text>低头</el-text>
              <el-slider v-model="headUpDown" :min="0" :max="80" :step="1" :marks="marks0_80"
                @change="updateCommandPreview" style="width: 200px; padding: 0 10px;"></el-slider>
              <el-text>抬头</el-text>
            </div>
          </div>
          <div class="col">
            <div class="header-title"><el-text style="padding-right: 10px;">舵机 1</el-text><el-switch v-model="servo1On" @change="onChangeSwitch"></el-switch></div>
            <div class="item">
              <el-text>左看</el-text>
              <el-slider v-model="headLeftRight" :min="0" :max="120" :step="1" :marks="marks0_120"
                @change="updateCommandPreview" style="width: 200px; padding: 0 10px;"></el-slider>
              <el-text>右看</el-text>
            </div>
          </div>
        </div>
      </el-form-item>

      <el-form-item label="手臂控制">
        <div class="col">
          <div class="col">

            <div class="header-title">
              <el-text>舵机 2（左手）</el-text><el-switch v-model="servo2On" @change="onChangeSwitch"></el-switch>
            </div>
            <div class="item">
              <el-text>放下</el-text>
              <el-slider v-model="leftArm" :min="0" :max="140" :step="1" :marks="marks0_140_reverse"
                :format-tooltip="reverseShowValue" @change="updateCommandPreview"
                style="width: 200px; padding: 0 10px;"></el-slider>
              <el-text>抬起</el-text>
            </div>
          </div>
          <div class="col">
            <div class="header-title">
              <el-text>舵机 3（右手）</el-text><el-switch v-model="servo3On" @change="onChangeSwitch"></el-switch>
            </div>
            <div class="item">
              <el-text>放下</el-text>
              <el-slider v-model="rightArm" :min="0" :max="140" :step="1" :marks="marks0_140"
                @change="updateCommandPreview" style="width: 200px; padding: 0 10px;"></el-slider>
              <el-text>抬起</el-text>
            </div>
          </div>

        </div>
      </el-form-item>

      <el-form-item label="身体控制">

        <div class="col">
          <div class="header-title">
            <el-text style="padding-right: 10px;">舵机 4</el-text><el-switch v-model="servo4On" @change="onChangeSwitch"></el-switch>
          </div>
          <div class="item">
            <el-text>左转</el-text>
            <el-slider v-model="bodyRotation" :min="0" :max="120" :marks="marks0_120" :step="1" style="width: 200px;"
              @change="updateCommandPreview"></el-slider>
            <el-text>右转</el-text>
          </div>
        </div>
      </el-form-item>
      <el-divider>其他选项</el-divider>
      <el-form-item label="">
        <div class="item" >
          <el-text style="padding-right: 10px;">速度</el-text>
          <el-select v-model="speed" @change="updateCommandPreview" style="width: 80px">
            <el-option :label="慢" :value="1">慢</el-option>
            <el-option :label="中" :value="3">中</el-option>
            <el-option :label="快" :value="6">快</el-option>
            <el-option :label="较快" :value="8">较快</el-option>
            <el-option :label="很快" :value="10">很快</el-option>
          </el-select>
          <el-text style="padding-right:10px; padding-left:30px; ">时延</el-text>
          <el-input-number v-model="interval" :min="0.02" :max="0.1" :step="0.02"
            @change="updateCommandPreview"></el-input-number>
        </div>  
      </el-form-item>

      <el-form-item label="">  
      </el-form-item>

      <el-form-item label="指令预览">
        <el-input type="textarea" :value="commandPreview" readonly></el-input>
      </el-form-item>
      <el-button type="primary" @click="onReset">复位</el-button>
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
      leftArm: 0, // 140 - value
      rightArm: 0,
      bodyRotation: 60,
      speed: 1,
      interval: 0.02,
      commandPreview: '',
      command: {},
      servo0On: true,
      servo1On: true,
      servo2On: true,
      servo3On: true,
      servo4On: true,
      marks0_80: {
        0: '0°',
        40: '40°',
        80: '80°'
      },
      marks0_120: {
        0: '0°',
        60: '60°',
        120: '120°'
      },

      marks0_140: {
        0: '0°',
        70: '70°',
        140: '140°'
      },
      marks0_140_reverse: {
        140: '0°',
        70: '70°',
        0: '140°'
      },
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
      const command = {
        name: "",
        speed: this.getSpeedString(this.speed),
        interval: this.interval
      };

      if (this.servo0On) {
        command.servo0 = this.headUpDown;
      }

      if (this.servo1On) {
        command.servo1 = this.headLeftRight;
      }

      if (this.servo2On) {
        let realLeftArmValue = 140 - this.leftArm;
        command.servo2 = realLeftArmValue;
      }

      if (this.servo3On) {
        command.servo3 = this.rightArm;
      }

      if (this.servo4On) {
        command.servo4 = this.bodyRotation;
      }

      this.commandPreview = JSON.stringify(command);
      this.command = command;
    },
    addAction() {
      this.$emit('add-action', JSON.parse(this.commandPreview));
    },
    reverseShowValue(value) {
      let newValue = 140 - value;
      return newValue.toString();
    },
    onChangeSwitch(value) {
      console.log(value);
      this.updateCommandPreview();
    },
    onReset() {
      // 复位
      this.headUpDown = 40;
      this.headLeftRight = 60;
      this.leftArm = 140;
      this.rightArm = 0;
      this.bodyRotation = 60;
      this.updateCommandPreview();
    }
  },
  mounted() {
    this.updateCommandPreview();
  }
};
</script>

<style>
.col {
  display: flex;
  flex-direction: column;
}

.header-title {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.item {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin-bottom: 20px;
}

.el-form-item {
  margin-bottom: 20px;
}
</style>
