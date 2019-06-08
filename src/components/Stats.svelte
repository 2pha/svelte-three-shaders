<script>
  import { onMount } from "svelte";
  import { onDestroy } from "svelte";

  let div;
  let fps = 0;
  let frames = 0;
  let beginTime = 0;
  let prevTime = 0;
  let _frameId = "";

  function end() {
    frames++;
    let time = (performance || Date).now();
    if (time >= prevTime + 1000) {
      let _fps = (frames * 1000) / (time - prevTime);
      prevTime = time;
      frames = 0;
      fps = _fps.toFixed(0);
    }
    return time;
  }

  function update() {
    beginTime = end();
  }

  function loop() {
    update();
    _frameId = window.requestAnimationFrame(() => {
      loop();
    });
  }

  onMount(async () => {
    beginTime = prevTime = (performance || Date).now();
    frames = 0;
    loop();
  });

  onDestroy(() => {
    window.cancelAnimationFrame(_frameId);
  });
</script>

<style>
  div {
    position: fixed;
    z-index: 100;
    padding: 10px;
    right: 0px;
    bottom: 0px;
    color: #fff;
  }
</style>

<div bind:this={div}>{fps} fps</div>
