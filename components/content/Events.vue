<script setup lang="ts">
import { TresCanvas, TresEvent } from '@tresjs/core'
import { BasicShadowMap, sRGBEncoding, NoToneMapping } from 'three'

import { OrbitControls } from '@tresjs/cientos'

const gl = {
  clearColor: '#202020',
  shadows: true,
  alpha: false,
  shadowMapType: BasicShadowMap,
  outputEncoding: sRGBEncoding,
  toneMapping: NoToneMapping,
}

function onClick(ev: TresEvent) {
  if (ev) {
    ev.object.material.color.set('#008080')
  }
}

function onPointerEnter(ev: TresEvent) {
  console.log(ev)
  if (ev) {
    ev.object.material.color.set('#CCFF03')
  }
}

function onPointerLeave(ev: TresEvent) {
  if (ev) {
    /*  ev.object.material.color.set('#efefef') */
  }
}
</script>

<template>
  <TresCanvas v-bind="gl">
    <OrbitControls />
    <TresPerspectiveCamera :position="[11, 11, 11]" :fov="45" :near="0.1" :far="1000" :look-at="[0, 0, 0]" />

    <template v-for="x in [-2.5, 0, 2.5]">
      <template v-for="y in [-2.5, 0, 2.5]">
        <TresMesh
          v-for="z in [-2.5, 0, 2.5]"
          :key="`${[x, y, z]}`"
          :position="[x, y, z]"
          @click="onClick"
          @pointer-enter="onPointerEnter"
          @pointer-leave="onPointerLeave"
        >
          <TresBoxGeometry :args="[1, 1, 1]" />
          <TresMeshToonMaterial color="#efefef" />
        </TresMesh>
      </template>
    </template>
    <TresDirectionalLight :intensity="1" />
    <TresAmbientLight :intensity="1" />
  </TresCanvas>
</template>
