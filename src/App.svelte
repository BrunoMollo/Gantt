<script lang="ts">
  let value=""
  let cut=null
  let matrix=[]
  let max_lenght=0 

  function parse(){
    try{
      const tasks= JSON.parse(value)
      for(let task of tasks){
        const row=[]
        for(let i=0;i<task.estart;i++){
          row.push("white")
        }
        for(let i=0;i<task.duracion;i++){
          const {htotal}=task
          row.push(htotal?"green":"red")
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
    catch(e){
      alert("Eso no es json valido pedazode gil")
    }
  }

  function reset(){
    matrix=[]
    max_lenght=0
    value=""
    cut=null
  }

</script>

<h1 >Gantt</h1>
<textarea bind:value={value} cols="55" rows="10" placeholder="json..."></textarea>
<br>
<input placeholder="control day" type="number" bind:value={cut} >
<br>
<button on:click={parse}>Generate</button>
<button on:click={reset}>Reset</button>
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
  {#each row as cell, j}
    <div class="cell"  style="background-color: {cell}; {(cut==j)?'border-left: 4px solid blue':''}" />
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
