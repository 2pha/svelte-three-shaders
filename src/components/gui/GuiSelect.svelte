<script>
  export let label = "";
  export let options = [];
  /*
  function handleSelect(e) {
    console.log(e.target.value);
  }
  */
</script>

<style>
  .gui-select {
    font-size: 11px;
    font-family: Arial, Helvetica, sans-serif;
    line-height: normal;
  }
  select {
    background-color: rgb(68, 68, 68);
    border: none;
    outline: none;
    color: rgb(0, 255, 170);
    font-size: 11px;
    line-height: 18;
    font-family: Arial;
    height: 18px;
    user-select: none;
    cursor: pointer;
  }
  .gui-item {
    border-bottom: 1px solid rgb(51, 51, 51);
    background-color: rgb(26, 26, 26);
    min-height: 24px;
    padding: 3px;
    box-sizing: border-box;
    display: flex;
    flex-flow: row wrap;
    align-items: center;
  }
  .gui-label {
    width: 96px;
    padding: 3px;
    color: rgb(255, 255, 255);
    cursor: default;
    user-select: none;
  }
  .gui-controller {
    width: 192px;
    color: rgb(0, 255, 170);
  }
</style>

<div class="gui-item gui-select">
  <div class="gui-label">{label}</div>
  <div class="gui-controller">
    <select on:change>
      <option>test</option>
      <option>test2</option>
    </select>
  </div>
</div>
