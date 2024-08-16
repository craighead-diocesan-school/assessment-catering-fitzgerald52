<script>
  import Header from '$lib/Header.svelte'
  let food = getFood()
  async function getFood() {
    let shopData = await fetch('https://digitech.craighead.school.nz/api/restaurant')
    return shopData.json()
  }
</script>

<Header />

<main>
  <h2>SvelteKit</h2>
  {#await food}
    <!-- gives users an errror message when the page is loading  -->
    waiting...
  {:then food}
    {#each food.breakfast as food}
      {food.item} {food.description} {food.price} <img src={food.img} alt="" />
    {/each}
  {/await}
  <p>Welcome to coding with SvelteKit, a modern JavaScript framework that makes it easy to code great apps.</p>
</main>

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>
