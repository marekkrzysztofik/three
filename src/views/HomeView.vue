<template>
  <canvas id="bg" />
</template>
<script setup lang="ts">
import * as THREE from 'three'
import { ref, onMounted } from 'vue'

const experience = ref<HTMLCanvasElement | null>(null)
const camera = new THREE.PerspectiveCamera(
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
onMounted(() => {
  const renderer = new THREE.WebGLRenderer({
    canvas: document.querySelector('#bg'),
  })
  // const renderer = new THREE.WebGLRenderer({
  //   canvas: experience.value as unknown as HTMLCanvasElement,
  //   antialias: true,
  // })
  renderer.setSize(window.innerWidth, window.innerHeight)
  renderer.render(scene, camera)
})
</script>
