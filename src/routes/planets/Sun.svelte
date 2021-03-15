<script>
  export let handleMouseDown;
  export let size;
</script>

<div
  class="planet sun"
  on:mousedown={handleMouseDown}
  style="transform: scale({$size})"
>
  <div class="planet sun-content">
    <div class="planet sun-fg" />
    <div class="planet sun-bg" />
    <div class="planet sun-bg sun-bg--2" />
  </div>
</div>

<style>
  @keyframes rotate {
    0% {
      transform: rotate(0deg) scale(1.4);
    }
    100% {
      transform: rotate(360deg) scale(1.4);
    }
  }
  @keyframes rotate2 {
    0% {
      transform: rotate(0deg) scale(0.9);
    }
    100% {
      transform: rotate(360deg) scale(0.9);
    }
  }
  .planet {
    border-radius: 50%;
    height: 100px;
    width: 100px;
    position: absolute;
  }
  .sun {
    position: fixed;
    top: calc(50% - 50px);
    left: calc(50% - 50px);
  }

  .sun-content {
    transform: scale(1);
  }

  .sun-fg {
    background: orange;
    animation: rotate2 0.005s infinite linear;
    background: orange;
    background: linear-gradient(
      25deg,
      rgba(236, 97, 8, 1) 0%,
      rgba(191, 21, 21, 1) 100%
    );
    filter: blur(10px);
    transform-origin: center;
    position: absolute;
    z-index: 0;
  }

  .sun-bg--2 {
    animation: rotate 1s infinite linear;
    background: rgb(236, 97, 8);
    background: linear-gradient(
      12deg,
      rgba(236, 97, 8, 1) 0%,
      rgba(191, 21, 21, 1) 100%
    );
    transform-origin: center;
    position: absolute;
    z-index: -1;
  }
</style>
