<script>
  export let type = "button";
  export let href = null;
  export let mode = null;
  export let color = null;
  export let disabled = false;
</script>

<style>
button,
a {
  font: inherit;
  border: 1px solid #FFFF00;
  background: #FFFF00;
  padding: 0.5rem 1rem;
  color: #000;
  border-radius: 5px;
  box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.26);
  cursor: pointer;
  text-decoration: none;
}

button:focus {
  outline: none;
}

button:hover,
button:active,
a:hover,
a:active {
  background: #00CED1;
  border-color: #FFC0CB;
  box-shadow: 1px 1px 8px rgba(77, 51, 51, 0.26);
}

button:disabled,
button:disabled:hover,
button:disabled:active {
  background: #ccc;
  border-color: #ccc;
  color: #959595;
  box-shadow: none;
  cursor: not-allowed;
}

.success {
  background: #00CED1;
  border-color: #00CED1;
}

.success:hover,
.success:active {
  background: #00CED1;
  border-color: #00CED1;
}

.outline {
  background: transparent;
  color: #C71585;
  box-shadow: none;
  border: 1px solid #FFC0CB;
}

.outline:hover,
.outline:active {
  background: #ffc7de;
  box-shadow: none;
}

.outline:disabled,
.outline:disabled:hover,
.outline:disabled:active {
  background: transparent;
  color: #ccc;
}

.outline.success {
  border-color: #00CED1;
  color: #00CED1;
}

.outline.success:hover,
.outline.success:active {
  background: #E0FFFF;
}
</style>

{#if href}
  <a href="{href}">
    <slot />
  </a>
{:else}
  <button class="{mode} {color}" {type} {disabled} on:click>
    <slot />
  </button>
{/if}