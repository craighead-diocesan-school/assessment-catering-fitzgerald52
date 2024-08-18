<script>
  import Header from '$lib/Header.svelte'
  let foods = getFoods()
  let menu = []
  async function getFoods() {
    let shopData = await fetch('https://digitech.craighead.school.nz/api/restaurant')
    return shopData.json()
  }
  function addToMenu(food) {
    menu = [...menu, food]
  }
</script>

<Header />

<main>
  {#await foods}
    <!-- gives users an errror message when the page is loading  -->
    waiting...
  {:then foods}
    {#each foods.breakfast as food}
      {food.item}
      {food.description}
      {food.price} <img src={food.img} alt="" />
      <button
        on:click={() => {
          addToMenu(food)
        }}
      >
        add item to menu</button
      >
    {/each}
    {#each foods.dinner as food}
      {food.item}
      {food.description}
      {food.price} <img src={food.img} alt="" />

      <button
        on:click={() => {
          addToMenu(food)
        }}
      >
        add item to menu</button
      >
    {/each}{#each foods.dessert as food}
      {food.item}
      {food.description}
      {food.price} <img src={food.img} alt="" />
      <button
        on:click={() => {
          addToMenu(food)
        }}
      >
        add item to menu</button
      >
    {/each}
    <!-- <button on:click={food.selected == true}></button> -->
  {/await}
  {#each menu as food}
    {food.item}
    {food.description}
    {food.price} <img src={food.img} alt="" />
  {/each}
</main>

<style>
</style>
