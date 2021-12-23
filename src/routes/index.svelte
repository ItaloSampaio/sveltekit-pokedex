<script context="module">
  export async function load({ page }) {
    const url = 'https://pokeapi.co/api/v2/pokemon?limit=150'
    const res = await fetch(url)
    const data = await res.json()
    const pokemon = data.results.map((data, index) => {
      return {
        id: index + 1,
        name: data.name,
        image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
          index + 1
        }.png`
      }
    })

    return { props: { pokemon } }
  }
</script>

<script>
  import PokeCard from '../components/poke_card.svelte'

  export let pokemon = []
  let searchTerm = ''
  let filteredPokemon = []

  $: {
    filteredPokemon = searchTerm
      ? pokemon.filter((poke) => poke.name.toLowerCase().includes(searchTerm.toLowerCase()))
      : [...pokemon]
  }
</script>

<svelte:head>
  <title>SvelteKit Pokedex</title>
</svelte:head>

<h1 class="text-4xl text-center my-8 uppercase">SvelteKit Pokedex</h1>
<input
  class="w-full rounded-md p-4 text-lg border-2 border-gray-200"
  type="text"
  bind:value={searchTerm}
  placeholder="Search Pokemon"
/>
<div class="py-4 grid gap-4 md:grid-cols-2 grid-cols-1">
  {#each filteredPokemon as poke}
    <PokeCard {poke} />
  {/each}
</div>
