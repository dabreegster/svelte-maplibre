<script lang="ts">
  import Popup from '$lib/Popup.svelte';
  import MapLibre from '$lib/MapLibre.svelte';
  import GeoJSON from '$lib/GeoJSON.svelte';
  import FillLayer from '$lib/FillLayer.svelte';
  import { mapClasses } from '../styles.js';
  import code from './+page.svelte?raw';
  import CodeSample from '$site/CodeSample.svelte';
  import states from '$site/states.json?url';
  import { hoverStateFilter } from '$lib/filters.js';

  let show = true;

  function onKeyDown(e: KeyboardEvent) {
    if (e.key == 't') {
      e.stopPropagation();
      show = !show;
    }
  }
</script>

<svelte:window on:keydown={onKeyDown} />

<label
  ><input type="checkbox" bind:checked={show} /> Include the source and layer (press <b>t</b> to toggle)</label
>

<MapLibre
  style="https://basemaps.cartocdn.com/gl/positron-gl-style/style.json"
  class={mapClasses}
  standardControls
  center={[-98.137, 40.137]}
  zoom={4}
  on:error={(e) => window.alert(e.detail.error)}
>
  {#if show}
    <GeoJSON id="states" data={states} promoteId="STATEFP">
      <FillLayer
        paint={{
          'fill-color': 'green',
          'fill-opacity': hoverStateFilter(0.5, 1.0),
        }}
        beforeLayerType="symbol"
        manageHoverState
      >
        <Popup openOn="hover" let:data>{data.properties.NAME}</Popup>
      </FillLayer>
    </GeoJSON>
  {/if}
</MapLibre>

<CodeSample {code} />
