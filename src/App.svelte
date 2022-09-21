<script lang="ts">

  interface NamedOption {
    name: string
    status: boolean
  }

  let fields: NamedOption[] = [
    {name: "Übernachtung", status: false} as NamedOption,
    {name: "Halbpension", status: false} as NamedOption,
    {name: "Halbpension plus", status: false} as NamedOption,
    {name: "Vollpension", status: false} as NamedOption,
    {name: "Vollpension plus", status: false} as NamedOption,
    {name: "All Inklusive", status: false} as NamedOption,
    {name: "All Inklusive plus", status: false} as NamedOption,
  ]

  let mouseDown: boolean = false;
  let mouseDownIndex: number;
  let mouseDownState: boolean = false;

  const doMouseEnter = (i: number) => {
    // console.log("Enter", i)
    if (mouseDown) {
      let min = Math.min(i, mouseDownIndex);
      let max = Math.max(i, mouseDownIndex);

      fields = fields.map((it, index) => {
        if(index >= min && index <= max) {
          return {...it, status: mouseDownState}
        }else{
          return it
        }
      })
      fields[i].status = mouseDownState;
    }
  }

  const doMouseDown = (i: number) => {
    mouseDownIndex = i;
    mouseDownState = !fields[i].status;
    fields[i].status = mouseDownState
    mouseDown = true;
    addEventListener('mouseup', doMouseUp)
    // console.log("Mouse down", mouseDown, "state", mouseDownState)
  }

  function doMouseUp() {
    mouseDown = false
    // console.log("Mouse down", mouseDown)
  }

  const doReset = () => {
    fields = fields.map((it) => {
      return {...it, status: false}
    })
  }
</script>

<main>
  <h1>Multirange</h1>

  <p>Try to click and drag</p>

  <h2>Minimalist <button on:click={doReset}>↺</button></h2>

  <fieldset>
    {#each fields as {name, status}, i}
      {#if status}
        <div on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)} class="on">
          <p>{name}</p>
        </div>
      {:else}
        <div on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)}>
          <p>{name}</p>
        </div>
      {/if}
    {/each}
  </fieldset>

  <h2>Checkbox <button on:click={doReset}>↺</button></h2>

  <fieldset>
    {#each fields as {name, status}, i}
      {#if status}
        <div on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)} class="on">
          <p>☑ {name}</p>
        </div>
      {:else}
        <div on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)}>
          <p>☐ {name}</p>
        </div>
      {/if}
    {/each}
  </fieldset>

  <h2>Indicator <button on:click={doReset}>↺</button></h2>

  <fieldset>
    {#each fields as {name, status}, i}
      {#if status}
        <div class="tooltip on" on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)}>
          <p >☑ {name}</p>
          <span class="tooltiptext">Click and drag for multi select</span>
        </div>
      {:else}
        <div class="tooltip" on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)}>
          <p >☐ {name}</p>
          <span class="tooltiptext">Click and drag for multi select</span>
        </div>
      {/if}
    {/each}
  </fieldset>

</main>

<style>
  fieldset {
    margin-bottom: 10px;
  }

  fieldset > div {
    display: flex;
    user-select: none;
  }

  fieldset > div:hover {
    background-color: #eef;
  }

  .on {
    background-color: #ddf;
  }

  /* Tooltip container */
  .tooltip {
    position: relative;
    /*display: inline-block;*/
    /*border-bottom: 1px dotted black; !* If you want dots under the hoverable text *!*/
  }

  /* Tooltip text */
  .tooltip .tooltiptext {
    visibility: hidden;
    width: 120px;
    background-color: #555;
    color: #fff;
    text-align: center;
    padding: 5px 0;
    border-radius: 6px;

    /* Position the tooltip text */
    position: absolute;
    z-index: 1;
    bottom: 125%;
    left: 50%;
    margin-left: -60px;

    /* Fade in tooltip */
    opacity: 0;
    transition: opacity 0.3s;
  }

  /* Tooltip arrow */
  .tooltip .tooltiptext::after {
    content: "";
    position: absolute;
    top: 100%;
    left: 50%;
    margin-left: -5px;
    border-width: 5px;
    border-style: solid;
    border-color: #555 transparent transparent transparent;
  }

  /* Show the tooltip text when you mouse over the tooltip container */
  .tooltip:hover .tooltiptext {
    visibility: visible;
    opacity: 1;
  }
</style>
