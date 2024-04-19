<!--
Copyright 2021 Google LLC

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

/**
* @author Rikard Lindstrom <rlindstrom@google.com>
*/
-->
<script>
  import Icon from "@/components/general/Icon.svelte";
  import CaptureGraphLine from "./CaptureGraphLine.svelte";
  import { imuDataColors } from "@/stores/ui/store";
  import { captureDataLength } from "@/stores/capture/store";
  import { dataLabels } from "@/stores/bleInterfaceStore/store.js";

  let sensorNames = [];

  export let data;
  export let label;
  export let filter = null;

  export let onDelete = () => {
    console.error("not implemented");
  };

  $: if ($dataLabels.length > 0) {
    sensorNames = $dataLabels;
  }
</script>

<div class="capture-graph">
  <span class="label h2">{label}</span>
  <button class="close-button icon-button" on:click={onDelete}
    ><Icon icon={"close_24px.svg"} /></button
  >
  {#each Array($captureDataLength) as _, index}
    <CaptureGraphLine
      {data}
      {index}
      color={filter && !filter[index]
        ? "rgba(225, 225, 230, 0.5)"
        : $imuDataColors[index % $imuDataColors.length]}
    />
  {/each}
</div>

<div class="legend">
  {#each sensorNames as name, index}
    <div class="legend-item">
      <svg width="16" height="16">
        <rect width="16" height="16" fill={$imuDataColors[index % $imuDataColors.length]} />
      </svg>
      <span class="legend-label">{name}</span>
    </div>
  {/each}
</div>

<style lang="scss">
  @import "@scss/vars";

  .capture-graph {
    // width: 233px;
    // height: 110px;

    background-color: $color-bg-tertiary;
    position: relative;

    .label,
    .close-button,
    :global(svg) {
      position: absolute;
    }

    .label {
      top: 4px;
      left: 7px;
      z-index: 1;
    }
    .close-button {
      top: 8px;
      right: 8px;
      display: none;
      z-index: 2;
    }

    &:hover {
      .close-button {
        display: block;
      }
    }

    :global(svg) {
      top: 0;
      left: 0;
    }
  }

  .legend {
    position: absolute;
    bottom: 0;
    right: 0;
    background-color: rgba(255, 255, 255, 0.8);
    padding: 10px;
    border-radius: 4px;
  }
  .legend-item {
    display: flex;
    align-items: center;
    margin-bottom: 5px;
  }
  .legend-label {
    margin-left: 5px;
    white-space: nowrap;
  }
</style>
