<script>
  import { createEventDispatcher } from "svelte";
  import GuiNumberRange from "./GuiNumberRange.svelte";

  const dispatch = createEventDispatcher();
  export let label = "";
  export let red = 0;
  export let green = 0;
  export let blue = 0;
  let expanded = false;
  $: closed = expanded ? "" : "closed";

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

  function toggleExpanded() {
    expanded = !expanded;
  }
</script>

<style>

</style>

<div class="gui-item gui-color">
  <div class="gui-label">{label}</div>
  <div class="gui-controller">
    <div
      on:click={toggleExpanded}
      class="gui-color-toggle"
      style="background-color: rgb({red}, {green}, {blue});">
       {red}, {green}, {blue}
    </div>
    <div class="gui-color-sliders {closed}">
      <GuiNumberRange
        label="red"
        on:change={e => {
          colorInputChange();
        }}
        bind:value={red}
        min="0"
        max="255"
        step="1.0" />
      <GuiNumberRange
        label="green"
        on:change={e => {
          colorInputChange();
        }}
        bind:value={green}
        min="0"
        max="255"
        step="1.0" />
      <GuiNumberRange
        label="blue"
        on:change={e => {
          colorInputChange();
        }}
        bind:value={blue}
        min="0"
        max="255"
        step="1.0" />
    </div>
  </div>
</div>
