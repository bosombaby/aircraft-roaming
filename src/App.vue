<template>
  <div class="control">
    <BaseFlightParams :data="flightParams" />
    <BaseMapPreview :data="flightParams" />
    <BaseTutorial />
  </div>
</template>
<script setup lang="ts">
import BaseMapPreview from "@/components/BaseMapPreview.vue"
import BaseFlightParams from "@/components/BaseFlightParams.vue"
import BaseTutorial from "@/components/BaseTutorial.vue"
import { useCesium } from "@/hooks/useCesium"
import { useUav } from "@/hooks/useUav"
import { onMounted, ref } from "vue"
import * as Cesium from "cesium"


const app = document.querySelector("#app") as HTMLElement
const { viewer } = useCesium(app)
const { flightParams } = useUav(
  viewer,
  `${import.meta.env.VITE_API_DOMAIN}/models/tb2.glb`
)

//设置地图
let tilesetUrls = [
  'http://218.92.67.67:9003/model/OxSgdqzA/tileset.json',
  'http://218.92.67.67:9003/model/mRkRKGqB/tileset.json',
  'http://218.92.67.67:9003/model/VziTuSKn/tileset.json',
]
let load3DTitles = () => {

  for (let i = 0; i < tilesetUrls.length; i++) {
    const tileset = new Cesium.Cesium3DTileset({
      url: tilesetUrls[i],
      maximumScreenSpaceError: 1,
      maximumNumberOfLoadedTiles: 1000
    })

    tileset.readyPromise.then((tileset) => {
      // adjustModelHeight(tileset, -9)
      viewer.scene.primitives.add(tileset)
    })
    // 显示3dtiles包围盒
    // tileset.debugShowContentBoundingVolume = true
    // viewer.zoomTo(tileset)
  }
}

// 调整模型位置
let adjustModelHeight = (tileset, height) => {
  // 显示3D Tiles包围盒
  // tileset.debugShowContentBoundingVolume = true

  const cartographic = Cesium.Cartographic.fromCartesian(
    tileset.boundingSphere.center
  );
  const surface = Cesium.Cartesian3.fromRadians(
    cartographic.longitude,
    cartographic.latitude,
    0.0
  );
  const offset = Cesium.Cartesian3.fromRadians(
    cartographic.longitude,
    cartographic.latitude,
    height
  );
  const translation = Cesium.Cartesian3.subtract(
    offset,
    surface,
    new Cesium.Cartesian3()
  );
  tileset.modelMatrix = Cesium.Matrix4.fromTranslation(translation);
}

//定位
let setCameraPos = () => {
  viewer.camera.setView({
    destination: new Cesium.Cartesian3(-2556423.687339962, 4558058.667541492, 3645710.7828753917),
    // destination: Cesium.Cartesian3.fromDegrees(101.50079, 108.65881, 1500),
    orientation: {
      heading: 5.95,
      pitch: -1.54,
      roll: 0
    },
  })
}


onMounted(() => {
  load3DTitles()
  // setCameraPos()
})




</script>
<style lang="scss" scoped>
.control {
  position: absolute;
  top: 10px;
  left: 10px;
  z-index: 9999;
  display: grid;
  grid-gap: 10px;
}
</style>
