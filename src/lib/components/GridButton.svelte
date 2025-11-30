<script lang="ts">
  import { BirdSvg, BlankSvg, SquareSvg, OneSvg, TwoSvg, ThreeSvg, FourSvg, FiveSvg, SixSvg, SevenSvg, EightSvg } from "$lib/vector";
  
  export let btnValue: -1 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8;

  let clicked: boolean = false;
</script>

<button class="cell-button" onclick={ () => clicked = true}>
    {#if btnValue === -1 && clicked}
        <BirdSvg />
    {:else if btnValue === 0 && clicked}
        <BlankSvg />
    {:else if btnValue === 1 && clicked}
        <OneSvg />
    {:else if btnValue === 2 && clicked}
        <TwoSvg />
    {:else if btnValue === 3 && clicked}
        <ThreeSvg />
    {:else if btnValue === 4 && clicked}
        <FourSvg />
    {:else if btnValue === 5 && clicked}
        <FiveSvg />
    {:else if btnValue === 6 && clicked}
        <SixSvg />
    {:else if btnValue === 7 && clicked}
        <SevenSvg />
    {:else if btnValue === 8 && clicked}
        <EightSvg />
    {:else}
        <SquareSvg />
    {/if}
</button>

<style>
    .cell-button {
        background-color: transparent;
        border: 0;
    }
</style>