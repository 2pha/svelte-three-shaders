<script>
  import { createEventDispatcher } from "svelte";

  import GuiSelect from "./gui/GuiSelect.svelte";
  import GuiNumberRange from "./gui/GuiNumberRange.svelte";

  const dispatch = createEventDispatcher();
  export let shapes = [];
  export let shaders = [];
  export let currentShader = {};

  $: shapeNames = shapes.map(shape => shape.name);
  $: shaderNames = shaders.map(shader => shader.name);
  $: customUniforms = currentShader.customUniforms || {};

  function handleShapeChange(e) {
    dispatch("shapeSelected", {
      shapeName: e.target.value
    });
  }

  function handleShaderChange(e) {
    dispatch("shaderSelected", {
      shaderName: e.target.value
    });
    console.log(customUniforms);
  }

  function numberUniformChange(e) {
    console.log(currentShader);
    currentShader.uniforms[e.detail.key].value = e.detail.value;
  }
</script>

<style>
  /* css in gloobal.css */
</style>

<div id="controls">
  <GuiSelect label="Shape" options={shapeNames} on:change={handleShapeChange} />
  <GuiSelect
    label="Shader"
    options={shaderNames}
    on:change={handleShaderChange} />
  {#each Object.entries(customUniforms) as [key, uniform]}
    {#if uniform.type == 'f' && !Boolean(uniform.hidden)}
      <GuiNumberRange
        label={key}
        bind:value={uniform.value}
        min={uniform.min}
        max={uniform.max}
        step={uniform.step}
        on:change={numberUniformChange} />
    {:else if uniform.type == 'c'}{/if}
  {/each}
</div>
