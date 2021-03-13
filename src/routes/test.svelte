<script>
  import { onMount } from "svelte";
  import axios from "axios";

  let planets = [];
  let error = null;

  onMount(async () => {
    try {
      axios.get("http://localhost:1337/planets").then((response) => {
        console.log("res", response.data);
        planets = response.data;
      });
    } catch (e) {
      error = e;
    }
  });
</script>

<main>
  <h1>Hello</h1>

  {#if error !== null}
    {error}
  {:else}
    <ul>
      {#each planets as planet}
        <li>
          <a href={`/${planet.name}`}>{planet.name}</a>
        </li>
      {/each}
    </ul>
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
