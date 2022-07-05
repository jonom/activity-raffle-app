<script lang="ts">
  import { createEventDispatcher } from "svelte";
  export let label = "";
  export let disabled = false;

  const dispatch = createEventDispatcher();

  function dispatchClick() {
    dispatch("click");
  }
</script>

<button on:click={dispatchClick} {disabled}>
  {label}
</button>

<style>
  button {
    font-family: inherit;
    font-size: inherit;
    padding: 1em 2em;
    color: #ff3e00;
    background-color: hsla(15, 100%, 90%);
    border-radius: 2em;
    border: 2px solid rgba(255, 62, 0, 0);
    outline: none;
    width: 200px;
    font-variant-numeric: tabular-nums;
    cursor: pointer;
  }

  button:focus {
    border: 2px solid #ff3e00;
  }

  button:active {
    background-color: hsla(15, 100%, 80%);
  }

  button:disabled {
    background-color: hsla(15, 0%, 90%);
    color: rgb(161, 161, 161);
  }
</style>
