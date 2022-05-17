<script lang="ts">
  export let number: number = 0;
  export let clickNo: number = 0;
  export let clickedNo: number = -1;
  export let gameOver = false;
  export let countingDown = false;
  let pressed = false;

  const onClick = (e) => {
    if (gameOver) {
      return;
    }
    if (!pressed) {
      clickNo += 1;
      clickedNo = clickNo;
    }
    if (clickedNo !== number) {
      gameOver = true;
    }
    pressed = true;
  };
</script>

<div
  class="flex w-24 h-24 lg:w-36 lg:h-36 bg-slate-200 rounded shadow items-center justify-center 
  {clickedNo === -1
    ? 'bg-slate-200 '
    : clickedNo === number
    ? 'bg-green-500'
    : 'bg-red-500'}"
  on:click={onClick}
>
  {#if pressed || gameOver || countingDown}
    <p class="font-semibold text-3xl">{number}</p>
  {/if}
</div>

<style lang="scss">
</style>
