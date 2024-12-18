<script>

    import { scaleLinear, scaleLog, scaleSqrt, scaleOrdinal } from 'd3-scale';
    import { extent } from 'd3-array';
    import data from '../lib/data.js'; 
    import Axis from '../lib/components/Axis.svelte';
    import Labels from '../lib/components/Labels.svelte';
    import RadialLegend from '../lib/components/RadialLegend.svelte';
  import CategoryLegend from '../lib/components/CategoryLegend.svelte';
  import Quadtree from '../lib/components/Quadtree.svelte';
    
    let sorted_data = data.sort((a, b) => b.gdp - a.gdp);
  
    const continents = [
      'North America',
      'South America',
      'Asia',
      'Europe',
      'Africa',
      'Oceania'
    ];
  
  
    let width;
    const height = 500;
    const margin = { top: 40, right: 20, bottom: 20, left: 35 };
  
    let xScale, yScale, radiusScale;
  
   
    $: if (sorted_data) {
      xScale = scaleLog()
        .domain(extent(sorted_data, (d) => +d.gdp / +d.population))
        .range([margin.left, width - margin.right]);
  
      yScale = scaleLinear()
        .domain(extent(sorted_data, (d) => +d.life_expectancy))
        .range([height - margin.bottom, margin.top]);
  
      radiusScale = scaleSqrt()
        .domain(extent(sorted_data, (d) => +d.population))
        .range([2, 20]);
    }
  
    const colors = scaleOrdinal()
      .range([
        '#e0dff7',
        '#EFB605',
        '#FFF84A',
        '#FF0266',
        '#45FFC8',
        '#FFC9C9',
        '#A8a1ff',
        '#2172FF',
        '#45FFC8'
      ])
      .domain([
        'All',
        'America',
        'Asia',
        'Europe',
        'South America',
        'Oceania',
        'Africa'
      ]);
  </script>
  
  
  
  <div style=" display:flex;max-width: 800px ;align-items: flex-start ;" bind:clientWidth={width}>npm run dev
    
  
    <div class="relative" style="background-color:#08143E;">
      {#if sorted_data && width}
  
      <Quadtree
      
      {data}
      {xScale}
      {yScale}
  {width}
  {height} 
  {margin}
  let:visible
  let:x
  let:y
  let:found >
  
  <div class="circle"
  style="top:{y}px;left:{x.circle}px;display:{visible ? 'block' : 'none'};
  width:{radiusScale(+found.population)*2 +5+
  0}px;height:{radiusScale(+found.population)*2 +5+0}px"
  
       />
       <div
       class="tooltip"
       style="top: {y + 5}px; left: {x.square + 10}px; display: {visible ? 'block' : 'none'}; background: rgba(255, 255, 255, 0.9); color: #1f2937; padding: 8px; border-radius: 4px; box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);">
       <h1 style="margin-bottom: 0.25rem; font-size: 1rem; color: #1f2937;">{found.country}</h1>
       GDP: {Number(found.gdp / 100000000).toFixed(1) + ' Bil. $'}<br />
       Pop.: {Number(+found.population / 1000000).toFixed(1) + ' Mil.'}<br />
       Life Expectancy: {Number(+found.life_expectancy).toFixed(1)}
   </div>
   
  
      </Quadtree>
  
  
  
        <svg {width} {height} > 
          <g>
            <Axis {width} {height} {margin} scale={xScale} position="bottom" />
            <Axis {width} {height} {margin} scale={yScale} position="left" />
            <Labels
      labelforx={true}
      {width}
      {height}
      {margin}
      yoffset={-30}
      xoffset={-170}
      label={'GDP per capita →'} />
    
    <Labels
      labelfory={true}
      textanchor={'start'}
      {width}
      {height}
      {margin}
      yoffset={10}
      xoffset={10}
      label={'Life Expectancy ↑'} />
  
           
            {#each sorted_data as d}
              <circle
                class={d.continent.split(' ').join('')}
                cx={xScale(+d.gdp / +d.population)}
                cy={yScale(+d.life_expectancy)}
                r={radiusScale(+d.population)}
                fill={colors(d.continent)} />
            {/each}
          </g>
            
        
        </svg>
        
      {/if}
    </div>
    <div>
      <svg width="150" height={height}> 
        <g transform="translate(-20, 300)">
          <RadialLegend {width} {height} {radiusScale} />
        </g>
  
        <g transform="translate(-20, 120)">
          <CategoryLegend legend_data={continents}
           legend_color_function={colors}
           SPACE={80} />
      </svg>
    </div>
  </div>
  
  
  
  <style>
    :global(body) {
      background-color:#08143E;
      margin: 0;
      padding: 0;
    }
  
    .circle {
      position: absolute;
      border-radius: 50%;
      transform: translate(-50%, -50%);
      pointer-events: none;
      width: 10px;
      height: 10px;
      border: 1px solid #fff;
      transition: left 300ms ease, top 300ms ease;
    }
    
    .tooltip {
      position: absolute;
      font-family: 'Poppins', sans-serif !important;
      min-width: 8em;
      line-height: 1.5;
      padding:8px;
      font-size: 0.875rem;
      opacity: 0.8;
      z-index: 1;
      border-radius: 4px;
      border:1px solid #d1d5db;
      background-color: #f9fafb;
      transition: left 100ms ease, top 100ms ease;
    }
  .country{
    color: white;
  }
  </style>