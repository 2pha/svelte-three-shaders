<script>
  import { createEventDispatcher } from "svelte";

  import GuiSelect from "./gui/GuiSelect.svelte";
  import GuiNumberRange from "./gui/GuiNumberRange.svelte";
  import GuiColor from "./gui/GuiColor.svelte";
  import GuiButton from "./gui/GuiButton.svelte";

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
  }

  function numberUniformChange(e) {
    currentShader.uniforms[e.detail.key].value = e.detail.value;
  }

  function colorUniformChange(e) {
    currentShader.uniforms[e.detail.label].value.setRGB(
      e.detail.value.red / 256,
      e.detail.value.green / 256,
      e.detail.value.blue / 256
    );
  }

  function codeButtonClick(e) {
    dispatch("codeButtonClick");
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
    <!-- Here we just use the key from customUniforms, but use the values from the original uniforms
    (as opposed to using the customUniforms directly unlike in the Vue and React examples),
    to do with how svelte seems to be making and updating references -->
    {#if uniform.type == 'f' && !Boolean(uniform.hidden)}
      <GuiNumberRange
        label={key}
        bind:value={currentShader.uniforms[key].value}
        min={currentShader.uniforms[key].min}
        max={currentShader.uniforms[key].max}
        step={currentShader.uniforms[key].step}
        on:change={numberUniformChange} />
    {:else if uniform.type == 'c' && !Boolean(uniform.hidden)}
      <GuiColor
        label={key}
        red={parseInt(currentShader.uniforms[key].value.r * 256, 10)}
        green={parseInt(currentShader.uniforms[key].value.g * 256, 10)}
        blue={parseInt(currentShader.uniforms[key].value.b * 256, 10)}
        on:change={colorUniformChange} />
    {/if}
  {/each}
  <GuiButton label="view shader code" on:click={codeButtonClick} />
</div>
