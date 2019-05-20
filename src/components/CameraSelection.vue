<template>
  <fieldset>
    <legend>カメラ</legend>
    <div>
      <label>
        <input type="radio" name="cameraType" value="facingMode" v-model="cameraType">
        facingMode
      </label>
      <select v-model="facingMode">
        <option>user</option>
        <option>environment</option>
      </select>
    </div>
    <div>
      <label>
        <input type="radio" name="cameraType" value="deviceId" v-model="cameraType">
        deviceId
      </label>
      <select v-model="deviceId">
        <option
          v-for="mediaDeviceInfo in mediaDeviceInfoList"
          v-bind:value="mediaDeviceInfo.deviceId"
          v-bind:key="mediaDeviceInfo.deviceId"
        >{{ mediaDeviceInfo.label }}</option>
      </select>
    </div>
    <div>
      Size:
      <label>
        <input type="radio" name="cameraSize" value="AUTO" v-model="cameraSize">AUTO
      </label>
      <label>
        <input type="radio" name="cameraSize" value="QVGA" v-model="cameraSize">QVGA
      </label>
      <label>
        <input type="radio" name="cameraSize" value="VGA" v-model="cameraSize">VGA
      </label>
      <label>
        <input type="radio" name="cameraSize" value="HD" v-model="cameraSize">HD
      </label>
      <label>
        <input type="radio" name="cameraSize" value="Full HD" v-model="cameraSize">Full HD
      </label>
      <label>
        <input type="radio" name="cameraSize" value="4K" v-model="cameraSize">4K
      </label>
    </div>
    <div>
      <button v-on:click="onClickSettingButton">設定</button>
    </div>
  </fieldset>
</template>

<script>
export default {
  name: "CameraSelection",
  data() {
    return {
      cameraType: "facingMode",
      facingMode: "environment",
      mediaDeviceInfoList: [],
      deviceId: null,
      cameraSize: "AUTO"
    };
  },
  mounted() {
    navigator.mediaDevices.enumerateDevices().then(infoList => {
      this.mediaDeviceInfoList = infoList.filter(
        info => info.kind == "videoinput"
      );
      if (this.mediaDeviceInfoList.length > 0) {
        this.deviceId = this.mediaDeviceInfoList[0].deviceId;
      }
    });
  },
  methods: {
    onClickSettingButton() {
      const cameraType = {};
      switch (this.cameraType) {
        case "facingMode":
          cameraType.type = "facingMode";
          cameraType.device = this.facingMode;
          break;
        case "deviceId":
          cameraType.type = "deviceId";
          cameraType.device = this.deviceId;
          break;
      }
      let cameraSize = null;
      switch (this.cameraSize) {
        case "AUTO":
          break;
        case "QVGA":
          cameraSize = { width: 320, height: 240 };
          break;
        case "VGA":
          cameraSize = { width: 640, height: 480 };
          break;
        case "HD":
          cameraSize = { width: 1280, height: 720 };
          break;
        case "Full HD":
          cameraSize = { width: 1920, height: 1080 };
          break;
        case "4K":
          cameraSize = { width: 4096, height: 2160 };
          break;
      }
      this.$emit("change-camera", cameraType, cameraSize);
    }
  }
};
</script>
