<template>
  <div id="app">
    <CameraSelection v-on:change-camera="onChangeCamera"/>
    <CameraCanvas ref="cameraCanvas" :cameraType="cameraType" :cameraSize="cameraSize"/>
  </div>
</template>

<script>
import CameraSelection from "./components/CameraSelection.vue";
import CameraCanvas from "./components/CameraCanvas.vue";

export default {
  name: "app",
  components: {
    CameraSelection,
    CameraCanvas
  },
  data() {
    return {
      cameraType: { type: "facingMode", device: "environment" },
      cameraSize: null
    };
  },
  methods: {
    onChangeCamera(cameraType, cameraSize) {
      this.cameraType = cameraType;
      this.cameraSize = cameraSize;
      this.$nextTick(() => this.$refs.cameraCanvas.updateCamera());
    }
  }
};
</script>
