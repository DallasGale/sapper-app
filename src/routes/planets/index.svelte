<script>
  import { onMount } from "svelte";
  import fetch from "node-fetch";
  // import ApolloClient, { gql } from "apollo-boost";

  // const planetQuery = gql`
  //   query planets {
  //     planets {
  //       id
  //       name
  //     }
  //   }
  // `;
  // export async function preload({ params, query }) {
  //   const client = new ApolloClient({
  //     uri: "http://localhost:1337/graphql",
  //     fetch: this.fetch,
  //   });
  //   const results = await client.query({
  //     query: planetQuery,
  //   });
  //   return { planets: results.data.planets };
  // }
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
</script>

<svelte:head>
  <title>articles</title>
</svelte:head>

<h1>recent posts</h1>

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
  ul,
  .main-title {
    font-size: 25px;
  }
</style>
