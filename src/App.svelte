<script lang="ts">
  interface Named {
    name: string
    status: string
  }

  let fields = [
    {name: "Aaaa aaa aaa", status: false},
    {name: "Bbbb bbb bbb", status: false},
    {name: "Cccc ccc ccc", status: true},
    {name: "Dddd ddd ddd", status: false},
    {name: "Eeee eee eee", status: false},
    {name: "Ffff fff fff", status: false},
    {name: "Gggg ggg ggg", status: false},
  ]

  let mouseDown : boolean = false;
  let mouseDownState : boolean = false;

  const doMouseEnter = (i:number) => {
    console.log("Enter", i)
    if(mouseDown) {
      fields[i].status = mouseDownState;
    }
  }

  const doMouseDown = (i:number) => {
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
</script>

<main>
  <h1>Multirange</h1>

  <fieldset>
    {#each fields as {name, status}, i}
      {#if status}
        <div on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)}>
          <p>☑ {name}</p>
        </div>
        {:else}
        <div on:mouseenter={() => doMouseEnter(i)} on:mousedown={() => doMouseDown(i)}>
          <p>☐ {name}</p>
        </div>
        {/if}
    {/each}
  </fieldset>

</main>

<style>
  fieldset > div {
    display: flex;
    user-select: none;
    background: var(--bg-color);
  }

  fieldset > div:hover {
    border: 2px solid lightblue;
  }

  fieldset > div > input {
    margin: 5px;
  }
</style>
