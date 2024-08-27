<script>
  import Header from '$lib/Header.svelte'
  import Card from '../lib/card.svelte'
  import food from '../lib/card.svelte'
  // defines variables
  let foods = getFoods()
  let menu = []

  let menuName = ''
  // gets information from provided website and stores it
  async function getFoods() {
    let shopData = await fetch('https://digitech.craighead.school.nz/api/restaurant')
    return shopData.json()
  }
  // adds item to menu and switches food.selected to true
  function addToMenu(food) {
    menu = [...menu, food]
    food.selected = true
    foods = foods
  }
  // removes item to menu and switches food.selected to false
  function removeFood(index, food) {
    menu = [...menu.slice(0, index), ...menu.slice(index + 1)]
    food.selected = false
    foods = foods
  }
</script>

<Header />
<!-- //alows users to name their custom menu -->
<p>input name of custom menu <input bind:value={menuName} /></p>

<main>
  <div class="columns">
    <div class="column">
      {#await foods}
        <!-- gives users an errror message when the page is loading  -->
        waiting...
      {:then foods}
        <!-- //goes through each of the foods and displays the information required -->
        {#each foods.breakfast as food}
          <div class:highlighted={food.selected}>
            <Card {food} />
            {#if !menu.includes(food)}
              <!-- //trigers add to menu function -->
              <button
                on:click={() => {
                  addToMenu(food)
                }}
              >
                add item to menu</button
              >
            {:else}
              <!-- //gives error message if it is in the menu -->
              <p>this item is already in your menu</p>
            {/if}
          </div>
        {/each}
        {#each foods.dinner as food}
          <!-- //the class is highlighted only if food.selected is true -->
          <div class:highlighted={food.selected}>
            <Card {food} />

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
          </div>
        {/each}
        {#each foods.dessert as food}
          <div class:highlighted={food.selected}>
            <!-- outputs  information from component -->
            <Card {food} />
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
          </div>
        {/each}
      {/await}
    </div>
    <div class="column">
      <!-- //displays the users input from before -->
      <h1>Custom Menu: {menuName}</h1>

      {#each menu as food, index}
        <Card {food} inMenu={true} />
        <!-- //trigers the remove food function -->
        <button
          on:click={() => {
            removeFood(index, food)
          }}
        >
          delete food</button
        >
      {/each}
      <!-- //gives error message if nothing is in the menu -->
      {#if menu.length == 0}
        <p>menu is empty, start adding food items</p>
      {/if}
    </div>
  </div>
</main>

<style>
  .highlighted {
    background-color: rgb(255, 163, 237);
  }
</style>
