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
  let mouseDownState: boolean = false;

  const doMouseEnter = (i: number) => {
    console.log("Enter", i)
    if (mouseDown) {
      fields[i].status = mouseDownState;
    }
  }

  const doMouseDown = (i: number) => {
    mouseDownState = !fields[i].status;
    fields[i].status = mouseDownState
    mouseDown = true;
    addEventListener('mouseup', doMouseUp)
    console.log("Mouse down", mouseDown, "state", mouseDownState)
  }

  function doMouseUp() {
    mouseDown = false
    console.log("Mouse down", mouseDown)
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

  <button on:click={doReset}>Reset</button>

  <h2>Minimalist</h2>

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

  <h2>Checkbox</h2>

  <fieldset>
    {#each fields as {name, status}, i}
      {#if status}
        <div on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)} class="on">
          <p>☑{name}</p>
        </div>
      {:else}
        <div on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)}>
          <p>☐{name}</p>
        </div>
      {/if}
    {/each}
  </fieldset>

</main>

<style>
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
</style>
