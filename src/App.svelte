<script lang="ts">

  let value = "";
  let cut = null;
  let matrix = [];
  let max_lenght = 0;

  function parse() {
    try {
      const tasks = JSON.parse(value);
      for (let task of tasks) {
        const row = [];
        for (let i = 0; i < task.estart; i++) {
          row.push("white");
        }
        for (let i = 0; i < task.duracion; i++) {
          const { htotal } = task;
          row.push(htotal ? "green" : "red");
        }
        max_lenght = task.efinish;
        matrix = [...matrix, row];
      }
      for (let row of matrix) {
        for (let i = row.length; i < max_lenght; i++) {
          row.push("white");
        }
      }
    } catch (e) {
      alert("Eso no es json valido pedazo de gil");
    }
  }

  function reset() {
    matrix = [];
    max_lenght = 0;
    value = "";
    cut = null;
  }


  const href="https://www.mercadopago.com.ar/money-out/transfer/new-account"
</script>

<h1>Gantt</h1>
<div style="">
  <ol>
    <li>Usar una navegador basado en Chome (no seas gil, firefox no)</li>
    <li>
      Entrar a <a href="https://creadorpertcpm.es/public/" target="_blank">esta pagina</a>
    </li>
    <li>Crear la Red de tareas</li>
    <li>Abrir la consola (Ctrl+Shift+I)</li>
    <li>Presionar el boton <b>Generar Diagrama</b></li>
    <li>En la consola se imprimar un array en formato json</li>
    <li>Copiar y pegar el json a continuacion</li>
  </ol>
</div>
<textarea bind:value cols="50" rows="10" placeholder="json..."></textarea>
<br />
<input placeholder="control day" type="number" bind:value={cut} />
<br />
<button on:click={parse} >Generar</button>
<button on:click={reset}>Reset</button>
<br /><br />

{#if max_lenght !== 0}
  <p>Se aceptan donaciones al siguiente alias: <b>jirafa.verde.drogada</b> 🦒
  (<a {href} on:click={(e)=>{
    e.preventDefault();
    navigator.clipboard.writeText("jirafa.verde.drogada")
    if(confirm("Alias copiado al portapapeles, ¿desea ir a mercado pago?")){
      window.open(href, '_blank').focus();
    }else{
      if(confirm("¿Enserio? creo que al menos me merezco una cerveza")){
         window.open(href, '_blank').focus();
      }
    }
  }}>Copiar</a>)
  </p>


  <main>
    <div class="row">
      <div class="cell"></div>
      {#each Array(max_lenght).fill(0) as _, i}
        <div class="cell">{i + 1}</div>
      {/each}
    </div>

    {#each matrix as row, i}
      <div class="row">
        <div class="cell">{i + 1}</div>
        {#each row as cell, j}
          <div
            class="cell"
            style="background-color: {cell}; {cut == j
              ? 'border-left: 4px solid blue'
              : ''}"
          />
        {/each}
      </div>
    {/each}
  </main>
  <p>App made by <a href="https://github.com/BrunoMollo?tab=repositories" target="_blank">Bruno Mollo</a></p>

{/if}

<a href="https://www.youtube.com/watch?v=q-Y0bnx6Ndw">Contactar a Soporte</a>
<style>
  .cell {
    display: inline-block;
    margin: 0;
    height: 20px;
    min-width: 24px;
    outline: 1px solid #111;
    background-color: #111;
  }
  .row {
    display: flex;
    align-items: start;
    padding: 0;
    margin: 0;
  }
</style>
