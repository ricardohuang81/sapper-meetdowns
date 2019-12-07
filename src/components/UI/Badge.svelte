<script>
  import { slide } from 'svelte/transition';
</script>
<style>
  span {
    display: inline-block;
    margin: 0 0.25rem;
    border-radius: 3px;
    border: 1px solid #FFFF00;
    background: #FFFF00;
    color: #1E90FF;
    padding: 0 0.5rem;
    font-family: 'Bebas Neue', cursive;
    font-size: 0.8rem;
  }

</style>

<span transition:slide>
  <slot />
</span>