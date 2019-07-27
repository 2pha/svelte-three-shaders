<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  export let label = "";
  export let value = 0;
  export let min = 0.0;
  export let max = 100;
  export let step = 0.1;

  function valueChange(e) {
    let newval = parseFloat(e.target.value);
    if (isNaN(newval)) {
      return;
    }
    if (newval < min) {
      newval = min;
    } else if (newval > max) {
      newval = max;
    }
    value = newval;
    dispatch("change", {
      key: label,
      value: newval
    });
  }
</script>

<style>
  /* css in global.css */
</style>

<div class="gui-item gui-number-range">
  <div class="gui-label">{label}</div>
  <div class="gui-controller">
    <input
      type="range"
      {value}
      {min}
      {max}
      {step}
      on:input|preventDefault={valueChange} />
    <input type="text" {value} on:change|preventDefault={valueChange} />
  </div>
</div>
