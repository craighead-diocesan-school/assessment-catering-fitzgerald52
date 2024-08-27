<script>
  export let food
  export let inMenu
  const gst = 0.15
</script>

{food.item}
{food.description}
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
