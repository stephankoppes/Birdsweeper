<!--
    Easy mode     :  8 x 8   10 mines
    Advanced mode : 16 x 16  40 mines
    Pro mode      : 30 x 16  99 mines
-->

<script lang="ts">
  import Bird from "$lib/assets/svg-bird.svelte";
  import One from "$lib/assets/svg-one.svelte";
  import Two from "$lib/assets/svg-two.svelte";
  import Three from "$lib/assets/svg-three.svelte";
  import Four from "$lib/assets/svg-four.svelte";
  import Five from "$lib/assets/svg-five.svelte";
  import Six from "$lib/assets/svg-five.svelte";
  import Seven from "$lib/assets/svg-five.svelte";
  import Eight from "$lib/assets/svg-five.svelte";
  
  let fieldCols = 8;
  let fieldRows = 8;
  let birdsTotal = 10;
  let adjacentBirds = 0;
  let birdLocations = CreateBirdLocations(fieldCols, fieldRows, birdsTotal);

  function ShowId(idName: string) {
    console.log(`You clicked cell ${idName}`);
  }

  function CreateBirdLocations(maxCol: number, maxRow: number, totalBirds: number) {
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
    // TO DO: Create function to find number of adjacent birds
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
              <button class="minefield-button" id="{row}-{col}" onclick={() => ShowId(`${row}-${col}`)}>
                {#if birdLocations.some(bird => bird.x == row && bird.y == col)}
                  <Bird />
                {:else}
                  {@const adjacentBirds = FindAdjacentBirds(row, col)}
                  {#if adjacentBirds == 1}
                    <One />
                  {:else if adjacentBirds == 2}
                    <Two />
                  {:else if adjacentBirds == 3}
                    <Three />
                  {:else if adjacentBirds == 4}
                    <Four />
                  {:else if adjacentBirds == 5}
                    <Five />
                  {:else if adjacentBirds == 6}
                    <Six />
                  {:else if adjacentBirds == 7}
                    <Seven />
                  {:else if adjacentBirds == 8}
                    <Eight />
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
    color: white;
    align-items: center;
    justify-content: center;
  }

  /* .field-number {
    font-family: Verdana, sans-serif;
    font-size: 16px;
    font-weight: 500;
  } */

  .minefield-button {
    background-color: transparent;
    border: 0;

  }
</style>
