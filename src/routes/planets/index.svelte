<script>
  import fetch from "node-fetch";
  import { spring } from "svelte/motion";

  // import Sun from "./Sun.svelte";
  // import Planet from "./Planet.svelte";

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

  let size = spring(1, {
    stiffness: 0.1,
    damping: 0.35,
  });

  let sizeTracker = 1;
  let active = false;
  const handleMouseDown = () => {
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
</script>

<svelte:head>
  <title>Planatery Motions</title>
</svelte:head>

<!-- <section class="canvas" class:active>
  <Planet name="earth" />
  <Sun {size} {handleMouseDown} />
</section> -->

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
  /* .canvas {
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
  } */

  ul,
  .main-title {
    font-size: 25px;
  }
</style>
