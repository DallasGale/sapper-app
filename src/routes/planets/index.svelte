<script>
  import fetch from "node-fetch";
  import { spring } from "svelte/motion";

  let planets;
  const token = "e0017ab4bdf7520a5060a938e297c5";

  fetch("https://graphql.datocms.com/", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
      Authorization: `Bearer ${token}`,
    },
    body: JSON.stringify({
      query: "{ allPlanets { name } }",
    }),
  })
    .then((res) => res.json())
    .then((res) => {
      planets = res.data.allPlanets;
      console.log("planets resp", planets);
    })
    .catch((error) => {
      console.log(error);
    });

  console.log("my planets from /planets:", planets);

  let coords = spring(
    { x: 50, y: 50 },
    {
      stiffness: 0.1,
      damping: 0.25,
    }
  );

  let size = spring(1, {
    stiffness: 0.1,
    damping: 0.35,
  });

  let sizeTracker = 1;
  let active = false;
  const handleMouseDown = () => {
    console.log("sizeTracker", sizeTracker);
    if (sizeTracker === 1) {
      size.set(2);

      sizeTracker = 2;
      active = !active;
      return;
    }

    if (sizeTracker === 2) {
      size.set(1);

      sizeTracker = 1;
      active = !active;
      return;
    }
  };

  console.log("active", active);
</script>

<svelte:head>
  <title>Planatery Motions</title>
</svelte:head>

<section class="canvas" class:active>
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
</section>
<ul>
  {#if planets}
    {#each planets as planet}
      <li>
        <a class="main-title" rel="prefetch" href="planets/{planet.name}">
          {planet.name}
        </a>
      </li>
    {/each}
  {/if}
</ul>

<style>
  .canvas {
    left: 0;
    background: rgb(223, 223, 223);
    position: absolute;
    top: 0;
    width: 100vw;
    transition: all 0.3s;
    height: 100vh;
  }
  .active {
    background: rgb(0, 0, 0);
    transition: all 0.3s;
  }
  .planet {
    border-radius: 50%;
    height: 1px;
    width: 1px;
    position: absolute;
  }
  .sun {
    position: fixed;
    top: 50%;
    left: 50%;
  }

  .sun-content {
    transform: scale(100);
  }

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
  .sun-fg {
    background: orange;
    animation: rotate2 3s infinite linear;
    /* background: rgb(236, 97, 8); */
    background: orange;
    background: linear-gradient(
      25deg,
      rgba(236, 97, 8, 1) 0%,
      rgba(191, 21, 21, 1) 100%
    );
    filter: blur(0.1px);
    transform-origin: center;
    position: absolute;
    z-index: 0;
  }
  /* .sun-bg {
    background: rgb(255, 94, 0);

    position: absolute;
    z-index: 0;
    transform: scale(1.2);
  } */

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

  ul,
  .main-title {
    font-size: 25px;
  }
</style>
