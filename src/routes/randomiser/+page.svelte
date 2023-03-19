<!-- ./randomiser.svelte -->
<script lang='ts'>
    let theValue: number = 40;
    function randomiser(){theValue = Math.floor(Math.random() * 100);}
  </script>
  <button on:click={randomiser}>random!</button><br>
  <p>the value: {theValue}</p>