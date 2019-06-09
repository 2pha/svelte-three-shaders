<script>
  import * as THREE from "three";
  import { onMount } from "svelte";

  let div,
    renderer,
    camera,
    scene,
    light,
    light1,
    pointLightHelper1,
    light2,
    pointLightHelper2,
    geometry,
    mesh;

  export let currentShape;
  export let currentShader;
  export let animateCallback = function() {};

  // how to simply react (call a function) when a specific property changes?
  // this does not seem right (though it works).
  $: if (currentShape) {
    addMesh();
  }
  $: if (currentShader) {
    mesh.material = currentShader;
  }

  onMount(async () => {
    setupScene();
    div.appendChild(renderer.domElement);
    sizeRenderer();
    animate();
  });

  function setupScene() {
    // Create renderer.
    renderer = new THREE.WebGLRenderer({ antialias: true });
    // Set the pixel ratio.
    // kills performance on high res monitors (especially my laptop)
    //renderer.setPixelRatio(window.devicePixelRatio);
    // Create camera.
    camera = new THREE.PerspectiveCamera(
      70,
      window.innerWidth / window.innerHeight,
      1,
      1000
    );
    // Position Camera.
    camera.position.z = 400;
    // Create Scene.
    scene = new THREE.Scene();
    // Create and add AmbientLight.
    light = new THREE.AmbientLight(0x404040); // soft white light
    scene.add(light);
    // Add rotating lights.
    light1 = new THREE.PointLight(0xff0000);
    scene.add(light1);
    pointLightHelper1 = new THREE.PointLightHelper(light1, 10);
    scene.add(pointLightHelper1);
    light2 = new THREE.PointLight(0x00ff00);
    scene.add(light2);
    pointLightHelper2 = new THREE.PointLightHelper(light2, 10);
    scene.add(pointLightHelper2);
    // Create initial Material.
    let material = new THREE.MeshBasicMaterial();
    // Create and add mesh/object.
    addMesh();
  }

  function addMesh() {
    // remove previous mesh.
    if (Boolean(mesh)) {
      scene.remove(mesh);
    }

    let shapeOb = currentShape;
    geometry = new THREE[shapeOb.class](...shapeOb.args);
    mesh = new THREE.Mesh(geometry, currentShader);
    // Check we have mounted and created a scene.
    if (typeof scene !== "undefined") {
      scene.add(mesh);
    }
  }

  function sizeRenderer() {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  }

  function animate() {
    let timer = Date.now() * 0.0005;
    mesh.rotation.x += 0.005;
    mesh.rotation.y += 0.01;
    // Check for props for light distance, or just put 400
    light1.position.x = Math.cos(timer) * 250;
    light1.position.z = Math.sin(timer) * 250;
    light2.position.y = Math.cos(timer * 1.25) * 250;
    light2.position.z = Math.sin(timer * 1.25) * 250;
    //this.$emit("animate");
    renderer.render(scene, camera);
    // call the animateCallback function.
    animateCallback();
    requestAnimationFrame(animate);
  }
</script>

<svelte:window on:resize={sizeRenderer} />
<div bind:this={div} />
