<script>
  import { onMount } from "svelte";

  import Scene from "./components/Scene.svelte";
  import Controls from "./components/Controls.svelte";
  import Stats from "./components/Stats.svelte";
  import CodeView from "./components/CodeView.svelte";

  import * as THREE from "three";

  import basicColor from "./shaders/BasicColor";
  import basicColorLights from "./shaders/BasicColorLights";
  import checker from "./shaders/Checker";
  import dots from "./shaders/Dots";
  import simpleLines from "./shaders/SimpleLines";
  import fadedLines from "./shaders/FadedLines";
  import starburst from "./shaders/Starburst";
  import matrix from "./shaders/Matrix";
  import normal from "./shaders/Normal";
  import voronoise from "./shaders/Voronoise";
  import woodGrain from "./shaders/WoodGrain";
  import simplexNoise3D from "./shaders/SimplexNoise3D";
  import perlinNoise3D from "./shaders/PerlinNoise3D";
  import perlinVertexDisp from "./shaders/PerlinVertexDisp";
  import polkaNoise from "./shaders/PolkaNoise";
  import fresnel2Color from "./shaders/Fresnel2Color";

  let shaders = [
    basicColor,
    basicColorLights,
    checker,
    dots,
    simpleLines,
    fadedLines,
    starburst,
    normal,
    matrix,
    voronoise,
    woodGrain,
    simplexNoise3D,
    perlinNoise3D,
    perlinVertexDisp,
    polkaNoise,
    fresnel2Color
  ];

  let shapes = [
    {
      name: "Cube",
      class: "BoxGeometry",
      args: [200, 200, 200, 50, 50, 50]
    },
    { name: "Sphere", class: "SphereGeometry", args: [150, 32, 32] },
    {
      name: "Cylinder",
      class: "CylinderGeometry",
      args: [100, 100, 200, 32, 100]
    },
    {
      name: "Torus Knot",
      class: "TorusKnotGeometry",
      args: [100, 30, 100, 16]
    }
  ];

  let threeVersion = THREE.REVISION;
  let clock = new THREE.Clock();

  let currentShader = {};
  let currentShaderObject = {};
  let currentShape = shapes[0];
  let showCode = false;

  onMount(async () => {
    setShaderFromName("Basic Color");
  });

  function getShaderFromName(name) {
    return shaders.find(x => x.name === name);
  }

  function setShaderFromName(name) {
    let shader = getShaderFromName(name);
    //create the options object to send to ShaderMaterial.
    let shaderObject = {
      vertexShader: shader.vertexShader,
      fragmentShader: shader.fragmentShader,
      lights: true
    };
    // Add uniforms if present.
    if ("uniforms" in shader) {
      // Using UniformUtils will clone the shader files uniforms,
      shaderObject.uniforms = THREE.UniformsUtils.merge([
        THREE.UniformsLib["lights"],
        shader.uniforms
      ]);
    }
    // Set this new material on the mesh.
    let material = new THREE.ShaderMaterial(shaderObject);
    // add the original uniforms here so we can loop over them in the Controls,
    // because other uniforms are added that we don't want controls for.
    material.customUniforms = shader.uniforms;

    currentShader = material;
    currentShaderObject = shader;
  }

  function getShapeFromName(name) {
    return shapes.find(x => x.name === name);
  }

  function changeShape(shapeName) {
    currentShape = getShapeFromName(shapeName);
  }

  function uniformChange(type, key, value) {
    if (type === "f") {
      currentShader.uniforms[key].value = value;
    } else if (type === "c") {
      currentShader.uniforms[key].value.setRGB(
        value.red / 256,
        value.green / 256,
        value.blue / 256
      );
    }
  }

  function animateCallback() {
    if (Boolean(currentShaderObject) && Boolean(currentShaderObject.update)) {
      currentShaderObject.update(currentShader.uniforms, clock);
    }
  }
</script>

<style>
  #info {
    padding: 10px;
    position: absolute;
    bottom: 0;
    left: 0;
    font-size: 12px;
    line-height: 16px;
    color: #fff;
  }
  #info a {
    color: #fff;
  }
</style>

<Scene {currentShape} {currentShader} {animateCallback} />
<Stats />
<Controls
  {shapes}
  {shaders}
  uniforms={currentShader.customUniforms}
  on:shapeSelected={e => {
    changeShape(e.detail.shapeName);
  }}
  on:shaderSelected={e => {
    setShaderFromName(e.detail.shaderName);
  }}
  on:codeButtonClick={e => {
    showCode = true;
  }}
  on:uniformChange={e => {
    uniformChange(e.detail.type, e.detail.key, e.detail.value);
  }} />
<div id="info">
  Three.js ShaderMaterial experiments.
  <br />
  Originals at
  <a
    target="_blank"
    rel="noopener noreferrer"
    href="https://2pha.com/blog/experimenting-threejs-shaders-and-shadermaterial/">
    this blog post
  </a>
  <br />
  Build with Three.js {threeVersion} and Svelte.js
  <br />
  <a
    target="_blank"
    rel="noopener noreferrer"
    href="https://github.com/2pha/svelte-three-shaders">
    https://github.com/2pha/svelte-three-shaders
  </a>
</div>
<CodeView
  bind:visible={showCode}
  shaderName={currentShaderObject.name}
  vertexShader={currentShaderObject.vertexShader}
  fragmentShader={currentShaderObject.fragmentShader} />
