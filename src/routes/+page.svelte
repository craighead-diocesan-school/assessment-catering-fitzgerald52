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
    food.selected = true
    foods = foods
  }
  function removeFood(index, food) {
    menu = [...menu.slice(0, index), ...menu.slice(index + 1)]
    food.selected = false
    foods = foods
  }
</script>

<Header />
<p>input name of custom menu <input bind:value={menuName} /></p>

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

          {food.price}
          <div class:highlighted={food.selected}><img src={food.img} alt="" /></div>
        {/each}
        {#each foods.dinner as food}
          {food.selected}
          {food.item}
          {food.description}
          {food.price}
          <div class:highlighted={food.selected}><img src={food.img} alt="" /></div>
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
        {/each}{#each foods.dessert as food}
          {food.item}
          {food.description}
          {food.price}
          <div class:highlighted={food.selected}><img src={food.img} alt="" /></div>
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
      {/await}
    </div>
    <div class="column">
      <h1>Custom Menu: {menuName}</h1>

      {#each menu as food, index}
        {food.item}
        {food.description}
        {food.price} <img src={food.img} alt="" />
        {(gst * food.price).toFixed(2)}

        <button
          on:click={() => {
            removeFood(index, food)
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
  .highlighted {
    background-color: aqua;
  }
</style>
