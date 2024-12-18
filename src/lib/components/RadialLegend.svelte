


   <script>
    import {onMount} from 'svelte';
    export let width, height, radiusScale;
    export let radialLegendData = [30000000, 300000000, 1000000000];
   let isReady=false;
   onMount(() => {
  const timeoutId = setTimeout(() => {
    isReady= true;
  }, 0);

  return () => clearTimeout(timeoutId);
});


  
    function format_number(d) {
      if (d < 1000000000) {
        const div = +d / +1000000 + 'M';
        return div;
      } else {
        const div = +d / +1000000000 + 'B';
        return div;
      }
    }
  </script>

<g fill="red" transform="translate(62,100)">
  {#if isReady}
    <text class="population-label" x="0" y="-42" text-anchor="middle">
      Population
    </text>
    {#each radialLegendData as d}
      <circle
        class="radial-legend-circle"
        cx="0"
        cy={10 - radiusScale(d)}
        r={radiusScale(d)} />

      <text class="value-label" x="40" y={13 - 2 * radiusScale(d)}>
        {format_number(d)}
      </text>

      <line
        class="legend-line"
        x1="0"
        x2="35"
        y1={10 - 2 * radiusScale(d)}
        y2={10 - 2 * radiusScale(d)} />
    {/each}
  {/if}
</g>

  
  <style>
    .population-label {
      fill: #E5E7EB;
      font-size: 1.125rem;
    }
    
    .radial-legend-circle {
      fill: transparent;
      stroke: #E5E7EB;
      stroke-dasharray: 2 2;
      box-sizing: border-box;
    }
  
    .value-label {
      fill: #E5E7EB;
      font-size: 0.75rem;
    }
  
    .legend-line {
      stroke: #D1D5DB;
      stroke-width: 1;
    }
  </style>
  