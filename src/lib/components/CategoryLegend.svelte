<script>
    export let legend_data, legend_color_function;
    import {onMount} from 'svelte';
    let isReady=false;
   onMount(() => {
  const timeoutId = setTimeout(() => {
    isReady= true;
  }, 0);

  return () => clearTimeout(timeoutId);
});
  </script>
  
  <style>
    .legend-text {
      fill: #e5e7eb;
      font-size: 1rem;
    }
  </style>
   {#if isReady}
  {#each legend_data as d, i}
    <rect
      x="25"
      y={i * 30 + 10}
      width="20"
      height="20"
      fill={legend_color_function(d)} />
    <text class="legend-text" x="60" y={25 + i * 30}>
      {d[0] + d.slice(1).toLowerCase()}
    </text>
  {/each}
  {/if}
  