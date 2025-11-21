<!--
    Easy mode     :  8 x 8   10 mines
    Advanced mode : 16 x 16  40 mines
    Pro mode      : 30 x 16  99 mines
-->

<script lang="ts">
  import Phoenix from "$lib/assets/icon-phoenix.svelte";
  import Bird from "$lib/assets/icon-bird.svelte";
  import One from "$lib/assets/icon-one.svelte";
  import Two from "$lib/assets/icon-two.svelte";
  import Three from "$lib/assets/icon-three.svelte";
  import Four from "$lib/assets/icon-four.svelte";
  import Five from "$lib/assets/icon-five.svelte";
  let fieldCols = 8;
  let fieldRows = 8;
  let birdsTotal = 10;
  // let birdLocations = ['1-3', '2-5', '4-4', '6-1', '0-7', '3-2', '5-5', '7-0', '2-2', '3-6'];
  let birdLocations = CreateBirdLocations(fieldCols, fieldRows, birdsTotal);

  function ShowId(idName: string) {
    console.log(`You clicked cell ${idName}`);
  }

  function CreateBirdLocations(maxCol: number, maxRow: number, totalBirds: number) {
    // TO DO: Create random bird locations based on birdsTotal variable
    let data = [];
    for (let i = 0; i < totalBirds; i++) {
      data.push({x: Math.floor(Math.random() * maxRow), y: Math.floor(Math.random() * maxCol)});
    }
    console.log('Random bird locations:', data);

    return data;
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
                  <p class="field-number">{row}-{col}</p>
                  <!-- TO DO: Run function to determine the number of adjacent birds
                              and display appropriate number icon -->
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
    border: 1px solid blue;
    background-color: cornflowerblue;
    color: white;
    align-items: center;
    justify-content: center;
  }

  .field-number {
    font-family: Verdana, sans-serif;
    font-size: 16px;
    font-weight: 500;
  }

  .minefield-button {
    background-color: transparent;
    border: 0;

  }
</style>
