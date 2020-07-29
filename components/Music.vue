<template>
  <h1>Music</h1>
</template>

<script>
import * as THREE from 'three/build/three.module'

import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'

export default {
  mounted() {
    let controls
    let camera, scene, renderer

    init()
    render()

    function init() {
      camera = new THREE.PerspectiveCamera(
        70,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      )
      camera.position.set(-1, 1, -1)

      scene = new THREE.Scene()

      const color = '0xFFFFFF'
      const intensity = 1
      const light = new THREE.DirectionalLight(color, intensity)
      light.position.set(0, 20, 0)
      light.target.position.set(-5, 0, 0)
      scene.add(light)
      scene.add(light.target)

      const loadertexture = new THREE.TextureLoader()
      const bgTexture = loadertexture.load('resources/images/daikanyama.jpg')
      scene.background = bgTexture

      const loader = new GLTFLoader()

      loader.load(
        '/gltf/record/record.gltf',
        function (gltf) {
          scene.add(gltf.scene)
        },
        undefined,
        function (error) {
          // eslint-disable-next-line no-console
          console.error(error)
        }
      )

      renderer = new THREE.WebGLRenderer({ antialias: true })
      renderer.setSize(window.innerWidth, window.innerHeight)
      renderer.outputEncoding = THREE.sRGBEncoding

      controls = new OrbitControls(camera, renderer.domElement)
      controls.addEventListener('change', render) // use if there is no animation loop
      controls.minDistance = 0
      controls.maxDistance = 10000
      controls.target.set(0, 0, -0.2)
      controls.update()

      document.body.appendChild(renderer.domElement)

      window.addEventListener('resize', onWindowResize, false)
    }

    function onWindowResize() {
      camera.aspect = window.innerWidth / window.innerHeight
      camera.updateProjectionMatrix()

      renderer.setSize(window.innerWidth, window.innerHeight)

      render()
    }

    function render() {
      renderer.render(scene, camera)
    }
  },
}
</script>

<style>
h1 {
  color: #fff;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 3rem;
  letter-spacing: 0.15em;
  position: absolute;
  text-transform: uppercase;
  top: 50%;
  transform: translate(-50%, -50%);
  right: 250px;
}
</style>
