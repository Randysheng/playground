<script setup lang="ts">
import { BasicShadowMap, sRGBEncoding, NoToneMapping } from 'three'
import { TresCanvas, useRenderLoop } from '@tresjs/core'

import { PamCameraMouse } from '@tresjs/cientos'

import Vertex from './shaders/vertex.glsl'
import Fragment from './shaders/fragment.glsl'

const gl = {
  clearColor: '#202020',
  shadows: true,
  alpha: false,
  shadowMapType: BasicShadowMap,
  outputEncoding: sRGBEncoding,
  toneMapping: NoToneMapping,
}

// Shaders

const shader = {
  vertexShader: Vertex,
  fragmentShader: Fragment,
  uniforms: {
    uTime: { value: 0 },
  },
}
</script>

<template>
  <TresCanvas v-bind="gl">
    <TresPerspectiveCamera :position="[0, 0, 5]" :fov="75" :near="0.1" :far="1000" />
    <PamCameraMouse :factor="2" />
    <TresMesh :scale="2" :position="[0.5, 0.5, 0]" cast-shadow>
      <TresSphereGeometry :args="[1, 30, 30]" />
      <TresShaderMaterial v-bind="shader" />
    </TresMesh>
    <TresAmbientLight :intensity="1" />
  </TresCanvas>
</template>
