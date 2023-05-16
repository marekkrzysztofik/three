<template>
  <canvas id="bg" />
</template>
<script setup lang="ts">
import * as THREE from 'three'
import { ref, onMounted, computed, watch } from 'vue'
import { useWindowSize } from '@vueuse/core'
let renderer: WebGLRenderer
let camera: PerspectiveCamera
const experience = ref<HTMLCanvasElement | null>(null)
const { width, height } = useWindowSize()
const aspectRatio = computed(() => width.value / height.value)
const updateRenderer = () => {
  renderer.setSize(width.value, height.value)
  renderer.setPixelRatio(window.devicePixelRatio)
}
const updateCamera = () => {
  camera.aspect = aspectRatio.value
  camera.updateProjectionMatrix()
}
watch(aspectRatio, updateRenderer)
watch(aspectRatio, updateCamera)

camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
)
const sphere = new THREE.Mesh(
  new THREE.SphereGeometry(1, 20, 20),
  new THREE.MeshBasicMaterial({ color: 0xff6397 })
)

camera.position.z = 5
const scene = new THREE.Scene()
scene.add(camera)
scene.add(sphere)
const loop = () => {
  renderer.render(scene, camera)
  sphere.position.y += 0.01;
  requestAnimationFrame(loop)
}
onMounted(() => {
  renderer = new THREE.WebGLRenderer({
    canvas: document.querySelector('#bg'),
  })
  // const renderer = new THREE.WebGLRenderer({
  //   canvas: experience.value as unknown as HTMLCanvasElement,
  //   antialias: true,
  // })
  updateRenderer()
  updateCamera()
  loop()
})
</script>
