<script>
  import { onMount } from "svelte";
  import { createEventDispatcher } from "svelte";

  import GuiSelect from "./gui/GuiSelect.svelte";

  const dispatch = createEventDispatcher();
  export let shapes = [];
  export let shaders = [];
  export let currentShader = {};

  $: shapeNames = shapes.map(shape => shape.name);
  $: shaderNames = shaders.map(shader => shader.name);
  $: customUniforms = currentShader.customUniforms || {};

  onMount(async () => {
    //gui = new dat.GUI({ autoPlace: false });
    //div.appendChild(gui.domElement);
  });

  function handleShapeChange(e) {
    dispatch("shapeSelected", {
      shapeName: e.target.value
    });
  }
</script>

<style>
  div {
    position: absolute;
    right: 0px;
  }
</style>

<div id="controls">
  <GuiSelect label="Shape" on:change={handleShapeChange} />
</div>
