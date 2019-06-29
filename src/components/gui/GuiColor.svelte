<script>
  import { createEventDispatcher } from "svelte";
  import GuiNumberRange from "./GuiNumberRange.svelte";

  const dispatch = createEventDispatcher();
  export let label = "";
  export let red = 0;
  export let green = 0;
  export let blue = 0;
  let expanded = false;

  function colorInputChange() {
    dispatch("change", {
      label: label,
      value: {
        red: red,
        green: green,
        blue: blue
      }
    });
  }
</script>

<style>

</style>

<div class="gui-item gui-color">
  <div class="gui-label">{label}</div>
  <div class="gui-controller">
    <div
      on:click={e => {
        expanded = !expanded;
      }}
      class="gui-color-toggle"
      style="background-color: rgb({red}, {green}, {blue});">
       {red}, {green}, {blue}
    </div>
    <div class="gui-color-sliders" class:closed={!expanded}>
      <GuiNumberRange
        label="red"
        on:change={colorInputChange}
        bind:value={red}
        min="0"
        max="255"
        step="1.0" />
      <GuiNumberRange
        label="green"
        on:change={colorInputChange}
        bind:value={green}
        min="0"
        max="255"
        step="1.0" />
      <GuiNumberRange
        label="blue"
        on:change={colorInputChange}
        bind:value={blue}
        min="0"
        max="255"
        step="1.0" />
    </div>
  </div>
</div>
