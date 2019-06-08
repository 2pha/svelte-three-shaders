<script>
  import Scene from "./components/Scene.svelte";
  import Controls from "./components/Controls.svelte";
  import Stats from "./components/Stats.svelte";

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

  function shapeChange(e) {
    console.log(e);
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

<Scene />
<Stats />
<Controls on:shapeSelected={shapeChange} />
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
