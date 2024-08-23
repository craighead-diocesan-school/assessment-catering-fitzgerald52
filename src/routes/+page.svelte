<script>
  import Header from '$lib/Header.svelte'
  let foods = getFoods()
  let menu = []
  let gst = 0.15
  let menuName = ''

  async function getFoods() {
    let shopData = await fetch('https://digitech.craighead.school.nz/api/restaurant')
    return shopData.json()
  }
  function addToMenu(food) {
    menu = [...menu, food]
  }
  function removeFood(index) {
    menu = [...menu.slice(0, index), ...menu.slice(index + 1)]
  }
</script>

<Header />

<main>
  <div class="columns">
    <div class="column">
      {#await foods}
        <!-- gives users an errror message when the page is loading  -->
        waiting...
      {:then foods}
        {#each foods.breakfast as food}
          {food.item}
          {food.description}
          {food.price} <img src={food.img} alt="" />
          {#if !menu.includes(food)}
            <button
              on:click={() => {
                addToMenu(food)
              }}
            >
              add item to menu</button
            >
          {:else}
            <p>this item is already in your menu</p>
          {/if}
        {/each}
        {#each foods.dinner as food}
          {food.item}
          {food.description}
          {food.price} <img src={food.img} alt="" />
          {#if !menu.includes(food)}
            <button
              on:click={() => {
                addToMenu(food)
              }}
            >
              add item to menu</button
            >
            <p>this item is already in your menu</p>
          {/if}
        {/each}{#each foods.dessert as food}
          {food.item}
          {food.description}
          {food.price} <img src={food.img} alt="" />
          {#if !menu.includes(food)}
            <button
              on:click={() => {
                addToMenu(food)
              }}
            >
              add item to menu</button
            >
            <p>this item is already in your menu</p>
          {/if}
        {/each}
      {/await}
    </div>
    <div class="column">
      <input bind:value={menuName} />

      {#each menu as food, index}
        {food.item}

        {food.description}
        {food.price} <img src={food.img} alt="" />
        {gst * food.price}
        <button
          on:click={() => {
            removeFood(index)
          }}
        >
          delete food</button
        >
      {/each}

      {#if menu.length == 0}
        <p>menu is empty, start adding food items</p>
      {/if}
    </div>
  </div>
</main>

<style>
</style>
