<!--
    Easy mode     :  8 x 8   10 mines
    Advanced mode : 16 x 16  40 mines
    Pro mode      : 30 x 16  99 mines

    Colors
    Unclicked cells : rgb(211, 211, 211) (lightgray)
    Clicked cells   : rgb(123, 163, 239) (light blue)

-->

<script lang="ts">
  import { BirdSvg, OneSvg, TwoSvg, ThreeSvg, FourSvg, FiveSvg, SixSvg, SevenSvg, EightSvg } from "$lib/vector";
  import GridButton from "./GridButton.svelte";
  
  let fieldCols = 8;    // X-col axis
  let fieldRows = 12;   // Y-row axis
  let birdsTotal = 10;
  let adjacentBirds = 0;
  let birdLocations = CreateBirdLocations(fieldCols, fieldRows, birdsTotal);

  function ShowCell(x: number, y: number) {
    console.log(`You clicked cell X:${x} Y: ${y}`);
  }

  function CreateBirdLocations(maxRow: number, maxCol: number, totalBirds: number) {
    let data: Array<{ x: number; y: number }> = [];
    for (let i = 0; i < totalBirds; i++) {
      let unique = false;
      while (!unique) {
        let newX = Math.floor(Math.random() * maxRow);
        let newY = Math.floor(Math.random() * maxCol);
        if (!data.some(bird => bird.x == newX && bird.y == newY)) {
          data.push({x: newX, y: newY});
          unique = true;
        }
      }
    }
    return data;
  }

  function FindAdjacentBirds(x: number, y: number) {
    let count = 0;
    
    // Check row (x) above
    if (y > 0) {
      if (birdLocations.some(bird => bird.x == x - 1 && bird.y == y - 1)) count++;
      if (birdLocations.some(bird => bird.x == x     && bird.y == y - 1)) count++;
      if (birdLocations.some(bird => bird.x == x + 1 && bird.y == y - 1)) count++;
    }

    // Check same row (x)
    if (birdLocations.some(bird => bird.x == x - 1 && bird.y == y)) count++;
    if (birdLocations.some(bird => bird.x == x     && bird.y == y)) count++;
    if (birdLocations.some(bird => bird.x == x + 1 && bird.y == y)) count++;

    // Check row (x) below
    if (birdLocations.some(bird => bird.x == x - 1 && bird.y == y + 1)) count++;
    if (birdLocations.some(bird => bird.x == x     && bird.y == y + 1)) count++;
    if (birdLocations.some(bird => bird.x == x + 1 && bird.y == y + 1)) count++;

    return count;
  }

</script>

<div class="main-container">
  <div class="game-container">
    <div class="tbl">
      {#each { length: fieldRows }, row}
        <div class="row">
          {#each { length: fieldCols }, col}
            <div class="cell">
              <button class="cell-button" id="{row}-{col}" onclick={() => ShowCell(col, row)}>
                {#if birdLocations.some(bird => bird.x == col && bird.y == row)}
                  <GridButton btnValue={-1} />
                {:else}
                  {@const adjacentBirds = FindAdjacentBirds(col, row)}
                  {#if adjacentBirds == 1}
                    <GridButton btnValue={1} />
                  {:else if adjacentBirds == 2}
                    <GridButton btnValue={2} />
                  {:else if adjacentBirds == 3}
                    <GridButton btnValue={3} />
                  {:else if adjacentBirds == 4}
                    <GridButton btnValue={4} />
                  {:else if adjacentBirds == 5}
                    <GridButton btnValue={5} />
                  {:else if adjacentBirds == 6}
                    <GridButton btnValue={6} />
                  {:else if adjacentBirds == 7}
                    <GridButton btnValue={7} />
                  {:else if adjacentBirds == 8}
                    <GridButton btnValue={8} />
                  {:else}
                    <GridButton btnValue={0} />
                  {/if}
                {/if}
              </button>
            </div>
          {/each}
        </div>
      {/each}
    </div>
  </div>
</div>


<style>
  .main-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .game-container {
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid blue;
    width: 800px;
    height: 800px;
  }

  .tbl {
    display: flex;
    flex-direction: column;
  }

  .row {
    display: flex;
    height: 60px;
    justify-content: center;
  }

  .cell {
    display: flex;
    width: 60px;
    border: 1px solid bisque;
    background-color: rgb(123, 163, 239);
    align-items: center;
    justify-content: center;
  }

  .cell-button {
    background-color: transparent;
    border: 0;
  }
</style>
