<script>
  export let visible = false;
  export let shaderName = "";
  export let vertexShader = "";
  export let fragmentShader = "";
  $: display = visible ? "block" : "none";
</script>

<div id="codeviewcontainer" style="display: {display}">
  <div id="closebar">
    <div id="bartitle">{shaderName}</div>
    <div
      id="closebutton"
      on:click={e => {
        visible = false;
      }}>
      Close
    </div>
  </div>
  <div id="codeview">
    <div class="pane">
      <div id="vertex-shader">
        <h3>Vertex Shader</h3>
        <pre>{vertexShader}</pre>
      </div>
    </div>
    <div class="pane">
      <div id="fragment-shader">
        <h3>Fragment Shader</h3>
        <pre>{fragmentShader}</pre>
      </div>
    </div>
  </div>
</div>
