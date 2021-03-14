<script context="module">
  import fetch from "node-fetch";

  const token = "e0017ab4bdf7520a5060a938e297c5";
  let planet = [];
  console.log("planet", planet);
  export async function preload({ params, query }) {
    const r = await fetch("https://graphql.datocms.com/", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
        Authorization: `Bearer ${token}`,
      },
      body: JSON.stringify({
        query: `query planet {
      allPlanets(filter: { name: { eq: ${params.slug} } }) {
        name
      
      }}`,
        variables: { slug: params.name },
      }),
    })
      .then((res) => res.json())
      .then((res) => {
        planet = res.data.allPlanets;
        console.log("planet");
        return planet;
      })
      .catch((error) => {
        console.log(error);
      });
  }
</script>

<svelte:head>
  <title>Planets</title>
</svelte:head>

{#each planet as p}
  <h1>{p.name}</h1>
{:else}
  {console.log(":else", planet)}
  waiting
{/each}
<p>⇺<a href="planets"> back to articles</a></p>

<!-- const results = await client.query({
      query: planetQuery,
      variables: { slug: params.slug },
    });
    console.log("results", results);
    return { planet: results.data.planets }; -->
<style>
</style>
