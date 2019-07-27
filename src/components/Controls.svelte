<script>
  import { createEventDispatcher } from "svelte";

  import GuiSelect from "./gui/GuiSelect.svelte";
  import GuiNumberRange from "./gui/GuiNumberRange.svelte";
  import GuiColor from "./gui/GuiColor.svelte";
  import GuiButton from "./gui/GuiButton.svelte";

  const dispatch = createEventDispatcher();
  export let shapes = [];
  export let shaders = [];
  //export let currentShader = {};
  export let uniforms = {};

  $: shapeNames = shapes.map(shape => shape.name);
  $: shaderNames = shaders.map(shader => shader.name);
  //$: customUniforms = currentShader.customUniforms || {};

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

  function uniformChange(e) {
    dispatch("uniformChange", e);
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
  {#each Object.entries(uniforms) as [key, uniform]}
    {#if uniform.type == 'f' && !Boolean(uniform.hidden)}
      <GuiNumberRange
        label={key}
        bind:value={uniform.value}
        min={uniform.min}
        max={uniform.max}
        step={uniform.step}
        on:valueChange={e => {
          uniformChange({
            type: uniform.type,
            key: key,
            value: e.detail.value
          });
        }} />
    {:else if uniform.type == 'c' && !Boolean(uniform.hidden)}
      <GuiColor
        label={key}
        red={parseInt(uniform.value.r * 255, 10)}
        green={parseInt(uniform.value.g * 255, 10)}
        blue={parseInt(uniform.value.b * 255, 10)}
        on:colorChange={e => {
          uniformChange({
            type: uniform.type,
            key: key,
            value: {
              red: e.detail.red,
              green: e.detail.green,
              blue: e.detail.blue
            }
          });
        }} />
    {/if}
  {/each}
  <GuiButton label="view shader code" on:click={codeButtonClick} />
</div>
