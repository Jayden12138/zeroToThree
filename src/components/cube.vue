<template>
  <div class="cube-container">
    <canvas width="400" height="400" id="three" />
  </div>
</template>

<script setup>
import * as THREE from "three";

import { onMounted } from "vue";

let cube = null;
let scene = null;
let renderer = null;
let camera = null;
let geometry = null;

function initScene() {
  scene = new THREE.Scene(); // 创建场景

  const fov = 90; // 视野范围（透视相机使用度，其他使用弧度）
  const aspect = 2; // 画布宽高比
  const near = 0.1; // 近平面
  const far = 5; // 远平面
  camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
  camera.position.z = 2;

  const canvas = document.getElementById("three");
  renderer = new THREE.WebGLRenderer({
    canvas, // 渲染的画布
    antialias: true, // 开启抗锯齿
  }); // 创建渲染器

  // renderer.setAnimationLoop(animate);

  const boxWidth = 1;
  const boxHeight = 1;
  const boxDepth = 1;
  geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth); // 创建立方体

  // const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 }); // 创建材质 // MeshBasicMaterial 不会受光影响

  const material = new THREE.MeshPhongMaterial({ color: 0x00ff00 }); // 创建材质

  // cube = new THREE.Mesh(geometry, material); // 创建网格（包含几何体、材质）

  // scene.add(cube); // 将网格添加到场景

  // renderer.render(scene, camera); // 将场景和相机传递给渲染器来渲染

  function render(time) {
    time *= 0.001; // 将时间单位变为秒

    // cube.rotation.x = time;
    // cube.rotation.y = time;

    cubes.forEach((cube, ndx) => {
      const speed = 1 + ndx * 0.1;
      const rot = time * speed;
      cube.rotation.x = rot;
      cube.rotation.y = rot;
    });

    renderer.render(scene, camera);

    requestAnimationFrame(render);
  }
  requestAnimationFrame(render);

  // 添加光照

  const color = 0xffffff;
  const intensity = 3;
  const light = new THREE.DirectionalLight(color, intensity);
  light.position.set(-1, 2, 4);
  scene.add(light);

  const cubes = [
    makeInstance(geometry, 0x44aa88, 0),
    makeInstance(geometry, 0x8844aa, -2),
    makeInstance(geometry, 0xaa8844, 2),
  ];
}

function makeInstance(geometry, color, x) {
  const material = new THREE.MeshPhongMaterial({ color });

  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);

  cube.position.x = x;

  return cube;
}

function animate() {
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;

  renderer.render(scene, camera);
}

onMounted(() => {
  initScene();
});
</script>
