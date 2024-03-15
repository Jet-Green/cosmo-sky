<script setup>
import { onMounted } from 'vue'
import gsap from 'gsap'

import * as THREE from 'three';


onMounted(() => {

  const scene = new THREE.Scene();

  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)

  // camera.position.z = -0.01
  // camera.position.x = -0.0002
  camera.position.y = 50

  const renderer = new THREE.WebGLRenderer({
    canvas: document.querySelector('#bg'),
  })

  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(window.innerWidth, window.innerHeight);

  // camera.position.setZ(30)


  // const geometry = new THREE.TorusGeometry(10, 3, 16, 100)
  // const material = new THREE.MeshStandardMaterial({ color: 0xff6347 })
  // const torus = new THREE.Mesh(geometry, material)
  // scene.add(torus)

  // const pointLight = new THREE.PointLight(0xffffff)
  // pointLight.position.set(0, 0, 0)
  // const lightHelper = new THREE.PointLightHelper(pointLight)
  // scene.add(pointLight, lightHelper)

  const ambientLight = new THREE.AmbientLight(0xffffff)
  scene.add(ambientLight)

  // const gridHelper = new THREE.GridHelper(200, 50)
  // scene.add(gridHelper)

  // const controls = new OrbitControls(camera, renderer.domElement)

  function addStar() {
    const geometry = new THREE.SphereGeometry(0.2, 24, 24)
    const material = new THREE.MeshStandardMaterial({ color: 0xffffff })
    const star = new THREE.Mesh(geometry, material)
    const [x, y, z] = Array(3).fill().map(() => THREE.MathUtils.randFloatSpread(200))

    star.position.set(x, y, z)
    scene.add(star)
  }

  for (let i = 0; i < 300; i++) {
    addStar()
  }
  // const spaceTexture = new THREE.TextureLoader().load('assets/images/space.jpg')
  // scene.background = spaceTexture

  function moveCamera() {
    const t = document.body.getBoundingClientRect().top
    gsap.to(camera.position, {
      x: t * -0.0002,
      y: t * 0.02,
      z: t * -0.01,
      duration: 2,
      ease: 'power3.out'
    })
    // camera.position.z = t * -0.01
    // camera.position.x = t * -0.0002
    // camera.position.y = t * 0.002
  }

  document.body.onscroll = moveCamera

  function animate() {
    requestAnimationFrame(animate)
    // torus.rotation.x += 0.01
    // torus.rotation.y += 0.005

    // controls.update()

    renderer.render(scene, camera)
  }

  animate()
})

</script>

<template>
  <div>
    <canvas id="bg"></canvas>
    <div class="main-text">
    </div>
  </div>
</template>

<style scoped>
canvas {
  position: fixed;
  top: 0;
  left: 0;
}

.main-text {
  position: absolute;
  height: 600vh;
  width: 100%;
}
</style>
