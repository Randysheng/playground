<script setup lang="ts">
import { TresCanvas, TresInstance, useRenderLoop, useTres } from '@tresjs/core'
import { BasicShadowMap, sRGBEncoding, NoToneMapping } from 'three'

import { TransformControls } from '@tresjs/cientos'
import { ShallowRef } from 'vue'

const gl = {
  clearColor: '#82DBC5',
  shadows: true,
  alpha: false,
  shadowMapType: BasicShadowMap,
  outputEncoding: sRGBEncoding,
  toneMapping: NoToneMapping,
}

const boxRef: ShallowRef<TresInstance | null> = shallowRef(null)

const { onLoop } = useRenderLoop()

onLoop(({ delta, elapsed }) => {
  if (boxRef.value) {
    boxRef.value.rotation.y += delta
    boxRef.value.rotation.z = elapsed * 0.2
  }
})

const camera = ref(null)

watchEffect(() => {
  if (camera.value) {
    camera.value.lookAt(0, 0, 0)
  }
})

const transformState = shallowReactive({
  mode: 'translate',
  size: 1,
  axis: 'XY',
  showX: true,
  showY: true,
  showZ: true,
})
</script>

<template>
  <TresCanvas v-bind="gl">
    <!--  <OrbitControls /> -->
    <TestOrbitControls make-default />
    <TresPerspectiveCamera ref="camera" :position="[3, 3, 3]" />
    <TresMesh ref="boxRef" :scale="1">
      <TresBoxGeometry :args="[1, 1, 1]" />
      <TresMeshNormalMaterial />
    </TresMesh>
    <TresAmbientLight :intensity="1" />
    <TresDirectionalLight :position="[0, 2, 4]" :intensity="2" />
  </TresCanvas>
</template>
