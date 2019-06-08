<script>
  import { onMount } from "svelte";

  let gui;

  export let currentShader;

  let div;

  onMount(async () => {
    //gui = new dat.GUI({ autoPlace: false });
    //div.appendChild(gui.domElement);
  });
</script>

<style>
  #controls {
    position: absolute;
    right: 0px;
  }
</style>

<div id="controls" bind:this={div} />
