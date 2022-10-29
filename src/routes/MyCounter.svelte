<div>
  {#if loop || decrementable}
    <button on:click={decrement}> - </button>
  {/if}

  <span> {count} </span>

  {#if loop || incrementable}
    <button on:click={increment}> + </button>
  {/if}
</div>

{#if count > 0}
  <div class="counter-label">
    {#each Array.from(new Array(count).keys()) as i}
      _
    {/each}
  </div>
{/if}

<script>
  import { getContext, hasContext } from 'svelte';

  const defaultMax = 100;
  const defaultMin = 0;
  const defaultLoop = true;

  export let min = defaultMin;
  export let max = defaultMax;
  export let loop = defaultLoop;

  let incrementable = true;
  let decrementable = false;

  let count = 0;

  $: {
    if (hasContext('minValue') && min === defaultMin) {
      min = getContext('minValue');
    }

    if (hasContext('maxValue') && max === defaultMax) {
      max = getContext('maxValue');
    }

    count = min;
  }

  $: increment = () => {
    if (count >= max) {
      count = min;
    } else {
      count += 1;
    }

    incrementable = !(count >= max);
    decrementable = (count >= max);
  };
  $: decrement = () => {
    if (count <= min) {
      count = max;
    } else {
      count -= 1;
    }

    decrementable = !(count <= min);
    incrementable = (count <= min);
  };
</script>

<style>
  button {
    border-radius: 8px;
    border: 1px solid transparent;
    padding: 0.6em 1.2em;
    font-size: 1em;
    font-weight: 500;
    font-family: inherit;
    background-color: #1a1a1a;
    cursor: pointer;
    transition: border-color 0.25s;
    color: white;
  }

  button:hover {
    border-color: #646cff;
  }

  button:focus,
  button:focus-visible {
    outline: 4px auto -webkit-focus-ring-color;
  }

  .counter-label {
    min-height: 20px;
  }

  @media (prefers-color-scheme: light) {
    button {
      background-color: #f9f9f9;
    }
  }
</style>