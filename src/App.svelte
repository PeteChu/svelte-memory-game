<script lang="ts">
  import './app.scss';
  import { differenceInSeconds, format, addSeconds } from 'date-fns';
  import Box from './lib/components/box.svelte';

  let clickCount = 0;
  let gameStarted = false;
  let gameOver = false;
  let unique = {};
  let countingDown = false;

  let countdownSeconds = -1;

  $: numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

  const start = () => {
    gameStarted = true;
    numbers = numbers
      .map((value) => ({ value, sort: Math.random() }))
      .sort((a, b) => a.sort - b.sort)
      .map(({ value }) => value);
    countingDown = true;

    // countdown
    let now = new Date();
    let gameStartIn = addSeconds(now, 10);
    let countdownTimer = setInterval(() => {
      now = new Date();
      countdownSeconds = differenceInSeconds(gameStartIn, now);
      if (countdownSeconds == 0) {
        clearInterval(countdownTimer);
        countingDown = false;
      }
    }, 100);
  };

  const reset = () => {
    unique = {};
    clickCount = 0;
    gameStarted = false;
    gameOver = false;
    numbers = [];
    numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    start();
  };
</script>

<main class="flex flex-col w-full h-full items-center justify-center">
  {#if !gameStarted || gameOver || clickCount === numbers.length}
    <div class="flex gap-2">
      {#if !gameOver}
        <button class="flex-1 my-3 p-2 bg-blue-400 rounded" on:click={start}
          >Start</button
        >
      {:else if gameOver}
        <button class="flex-1 my-3 p-2 bg-red-300 rounded" on:click={reset}
          >Retry</button
        >
      {/if}
    </div>
  {/if}

  <div>
    {#if countdownSeconds > 0}
      <p class="text-2xl">
        Game start in <span class="font-bold">{countdownSeconds}</span>
      </p>
    {:else if gameStarted && (!(clickCount === numbers.length) || gameOver)}
      <p class="text-2xl">Game started</p>
    {/if}
  </div>

  <div
    class="grid grid-cols-3 gap-1 {!gameStarted || gameOver
      ? 'pointer-events-none'
      : ''}"
  >
    {#key unique}
      {#each numbers as number}
        <Box
          {number}
          bind:clickNo={clickCount}
          bind:gameOver
          bind:countingDown
        />
      {/each}
    {/key}
  </div>
  <div class="status">
    {#if gameOver}
      <p class="my-3 text-3xl font-bold text-red-500">Game Over!</p>
    {/if}
    {#if clickCount === numbers.length}
      <p class="my-3 text-3xl font-bold text-green-500">You win!</p>
    {/if}
  </div>
</main>

<style lang="scss">
</style>
