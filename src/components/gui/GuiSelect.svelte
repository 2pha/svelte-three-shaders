<script>
  export let label = "";
  export let options = [];
</script>

<style>
  /* css in gloobal.css */
</style>

<div class="gui-item gui-select">
  <div class="gui-label">{label}</div>
  <div class="gui-controller">
    <select on:change>
      {#each options as option}
        <option>{option}</option>
      {/each}
    </select>
  </div>
</div>
