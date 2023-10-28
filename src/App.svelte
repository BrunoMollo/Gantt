<script lang="ts">
    import { fade } from "svelte/types/runtime/transition";


  let value=""
  let matrix=[]
  let max_lenght=0 

  function parse(){
    const tasks= JSON.parse(value)
    for(let task of tasks){
      let row=[]
      for(let i=0;i<task.estart;i++){
        row.push("white")
      }
      for(let i=0;i<task.duracion;i++){
        if(task.hlibre===0){
          row.push("red")
        }
        else{
          row.push("green")
        }
      }
      max_lenght=task.efinish
      matrix=[...matrix,row]; 
    }
    for(let row of matrix){
      for (let i=row.length;i<max_lenght;i++){
        row.push("white")
      }
    }
  }

</script>

<h1>Gannt</h1>
<textarea bind:value={value} cols="30" rows="10"></textarea>
<button on:click={parse}>click</button>
<button on:click={()=>matrix=[]}>reset</button>
  <br><br>

{#if max_lenght!==0}
<main>
<div class="row">
  <div class="cell"></div>
  {#each Array(max_lenght).fill(0)  as  _, i }
    <div class="cell">{i+1}</div>
  {/each}
</div>

{#each matrix as row, i }
<div class="row">
  <div class="cell">{i+1}</div>
  {#each row as cell}
    <div class="cell" style="background-color: {cell};" />
  {/each}
</div>
{/each}
</main>
{/if}

<style>
  .cell{
    display: inline-block;
    margin: 0;
    height: 20px;
    width: 20px;
    border-left: 1px solid #111;
    border-bottom: 1px solid #111;
  }
  .row{
    display: flex;
    align-items: start;
    padding: 0;
    margin:0;
    background-color: black;
  }
</style>
