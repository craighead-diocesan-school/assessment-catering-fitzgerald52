<script>
  // gets information from other pages
  export let food
  export let inMenu
  const gst = 0.15
</script>

<!-- gathers all information needed to be displyed into a component -->
{food.item}
{food.description}
<!-- only shows gst if item is in menu -->
{#if inMenu == true}
  $ {food.price.toFixed(2)}+{(gst * food.price).toFixed(2)}
{:else}
  $ {food.price.toFixed(2)}
{/if}
<img src={food.img} alt="" />

<style>
  img {
    padding: 5px;
  }
</style>
